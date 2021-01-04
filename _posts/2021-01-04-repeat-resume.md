---
title: |
  Repeatable and resumable simulations in Rust
author: 
layout: post
---

I’ve been running simulations that rely on random numbers, and I’m chasing two desirable properties. First, if I run the same code with the same arguments, I want the same results (whenever possible). And I want interrupted runs to pick up from where they left off. Repeatable and resumable code. These are my notes on the progress I’ve made, using the Rust programming language.

<!-- break -->

# Repeatable

There are four tricks here:

1. Setting a random seed.
2. Knowing the arguments you gave a program.
3. Knowing the software version you ran.
4. Using a predictable hash map (if you use them).

Plus, there’s one complication: as soon as you’re running on multi-core and multi-server environments, you’re at the whim of the scheduler. For my needs, there’s not a lot worth doing about this, other than being statistically repeatable.

I’ll give brief examples of the tricks, before discussing resumable programs.

## 1. Setting the random seed
The rand crate in Rust is [the library]([https://rust-random.github.io/book/intro.html]) for generating random numbers. Any random number algorithm that implements the `SeedableRng` trait can have its initial state set:

```rust
use rand::{SeedableRng, Rng, rngs::StdRng};
fn main() {
    let seed = 9998;
    let mut rng: StdRng = SeedableRng::seed_from_u64(seed);
    println!("With seed {}, the first random u8 is: {}", seed, rng.gen::<u8>());
}
```

By creating a random number generator with a fixed seed value, this code will produce the same “random” number every time it is run (unless we change the value of `seed`). You can [run this code in the Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=e152ebe396c7c6404ce87c4845094643).

## 2. Arguments
I don’t want to edit and recompile the code to change a seed. To provide the seed as an argument, I’ve been using [the structopt crate](https://docs.rs/structopt/). For example:

```rust
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct Settings {
    #[structopt(short, long)]
    seed: u64,
}

fn main() {
    let settings = Settings::from_args();
    println!("Running with seed: {}", settings.seed);
}
```

Here I’m gathering all the command line arguments (OK, the one argument) into a struct using structopt’s `from_args`. Running this code produces:

```
error: The following required arguments were not provided:
    --seed <seed>

USAGE:
    structopt-example --seed <seed>

For more information try --help
```

This gives me a way to pass a value into the program. During development, I’d pass this to the build tool, using a `--` to separate build arguments from program arguments: `cargo run -- --seed 1`.

I usually start a program by printing all the settings. For example:

```rust
println!("{:#?}", settings);
```

gives nicely formatted output of the struct:

```rust
Settings {
    seed: 1,
}
```

## 3. Software version
The settings are one factor in controlling a repeatable execution, but the other big one is the software itself. To record that I log the git version information using the [git-version crate](https://docs.rs/git-version/0.3.4/git_version/):

```rust
use git_version::git_version;
const GIT_VERSION: &str = git_version!();
fn main() {
    println!("{}", GIT_VERSION);
}
```

This is a macro, meaning it is capturing the git version information at compile time. That’s exactly what we’d want. Running the above code will produce something like: `2a5e159`. And that is something you can use to track down the exact commit that made up the programme (in this case, `https://github.com/d6y/structopt-example/commit/2a5e159`).

## Bonus: an experiment ID
Putting arguments and version information together gives as useful unique ID for a run of the software. For example, we can hash the arguments structure and combine it with the git version information to give an `experiment_id`:

```rust
fn experiment_id(settings: &Settings, version: &str) -> String {
    let settings_text = format!("{:?}", &settings);
    format!("{}_{}", version, hash(settings_text))
}

fn hash(s: String) -> u64 {
    use std::collections::hash_map::DefaultHasher;
    use std::hash::Hasher;

    let mut h = DefaultHasher::new();
    h.write(s.as_bytes());
    h.finish()
}


fn main() {
    let settings = Settings::from_args();
    let id = experiment_id(&settings, GIT_VERSION);
    println!("{}", id);

}
```

This program produces: `14e6520_9730971012316675531`. Hashing the arguments (the part after the underscore) is not great from a readability point of view, but it is good for log filenames, folder names, and is easily grep-able.

I usually throw in a timestamp too.

## 4. Predicable hash map
We know what our program state and arguments are, and we can control the random number generator. That’s mostly what I need, but one gotcha for me was that Rust uses a secure hash map implementation by default. 

Where this bit me is in a very narrow situation: storing keys and values in a map, where I need to select by a value, but having to deal with ties in the value. In this case, the security of the hash map means the key ordering won’t be deterministic.

An example will make that clearer.  Let’s say I want to find the most popular class in a list of examples. Specifically, suppose the examples are letters and I want to find the most frequent one:

```rust
use std::collections::HashMap;
fn main() {

    let examples = vec!["a", "b", "b", "c", "c"];

    // Update a map from letter -> number of occurances:
    let mut class_count = HashMap::new();
    for example in examples {
        *class_count.entry(example).or_insert(0) += 1;
    }

    println!("Counts: {:?}", class_count);

    let most_frequent_class: Option<&str> = class_count
        .into_iter()
        .max_by_key(|&(_, count)| count)
        .map(|(class, _)| class);

    println!("Most frequent: {:?}", most_frequent_class);
}
```

Running this code always produces the correct counts:

```rust
Counts: {"a": 1, "c": 2, "b": 2}
```

However, the most frequent will be either `Some("b")` or `Some("c")`depending on the run.  That’s fair, given both “a” and “b” have the same frequency, but it means the runs of my program won’t be reproducible with the same settings and source code.

There are a few ways to resolve this, but the point is to highlight that the default hash map is not deterministic in this respect. You can get a deterministic hasher from [the rustc-hash crate](https://crates.io/crates/rustc-hash). The change is to how I construct the hash map in the first place:

```rust
use rustc_hash::FxHasher;
use std::hash::BuildHasherDefault;
let mut class_count = HashMap::with_hasher(BuildHasherDefault::<FxHasher>::default());
```

With that, the hash map, `class_count`, is deterministic (and, as a bonus, extra fast).

# Resumable
Servers go away. Networks drop. Machines get restarted. There are lots of reasons why a run of a program may be interrupted. I don’t want to waste the effort that went into a partial run: I want to resume from where the program left off.

Given everything above, we’re in a good place to make any given run resumable. We can persist the software version and settings, giving a huge range of options for resuming a program:

- we can serialise settings to disk, and deserialise to resume; and we could
- persist a software version, so we can compare versions, perhaps refusing to start if versions differ.

The final part is to serialise the state of the random number generator. The idea is that if I restart a program, I’d want to resume and run to termination as if it had never been interrupted. Re-setting the seed would not do that. I need to reset the _state_ of the random number generator.

The [Rust user group pointed me at a random number generator](https://users.rust-lang.org/t/saving-and-restoring-the-state-of-a-seedablerng/52642/2?u=d6y%0A) with access to the internal state. The [rust-pcg crate](https://crates.io/crates/rand_pcg) implements the serialisation traits from serde, making saving and resuming state straightforward:

```rust
use rand::prelude::{Rng, SeedableRng};
use rand_pcg::Pcg32;
use serde_json;

let mut rng = Pcg32::seed_from_u64(seed);

// Convert to JSON:
let state = serde_json::to_string(&rng);

// And back again:
let mut restored_rng: Result<Pcg32, _> = 
  state.and_then(|json| serde_json::from_str(&json));
```

I’ve put together a runnable example of restoring state over at [https://github.com/d6y/restore-rand](https://github.com/d6y/restore-rand). It uses the experiment ID as a filename for saving and restoring state.

# Summary
With care, it’s possible to implement stochastic algorithms which can include elements of repeatability (to replicate results). The same principles then allow you to write robust code that can resume from failure.

This post has listed out the tricks I’ve learned for this, so far.



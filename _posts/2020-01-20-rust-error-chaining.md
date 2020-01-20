---
layout: post
title: "Combining differing error types"
author: "Richard Dallaway"
---

Rust, Scala, and many other languages let you use a kind of _or_ to represent errors.
In Scala it might be `Either<E, T>`, and in Rust it's likely to be `Result<T, E>`.
The `E` represents an error, and the awkward part of this is chaining together results with different types for `E`.
This post contains my notes on this, for Rust.

<!-- break -->

# Outline

- A short introduction to the Result type
- Combining errors of the same type
- Introducing `?` for short-cutting
- A mapping between errors of different type
- Application-specific errors
- The `From` type class for conversion between error types
- The standard error trait

Links and further reading are all at the end, except for links to the Rust playground to try out the code I'm showing.

# Result and its combinators

Converting strings into numbers is something that can go wrong. For example, trying to parse the string "bananas" as an unsigned 16-bit integer in Rust would fail:

```
fn main() {

  let number = "123".parse::<u16>();
  println!("{:?}", number);

  let error = "bananas".parse::<u16>();
  println!("{:?}", error);
}
```

Running that code ([open in playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=70456a980852aec16f5e56594d31ccc7)) outputs:

```
Ok(123)
Err(ParseIntError { kind: InvalidDigit })
```

Rust encourages you to encode these recoverable errors by using  `Result`. Both `number` and `error` have the same type: a `Result<u16, ParseIntError>`. I read this as "the result is either a u16 or a parse error".

As we can see, a `Result` can have a value that is either `Ok` or an `Err`:

```
pub enum Result<T, E> {
   Ok(T),
   Err(E),
}
```

You can pattern match on results:

```
let msg = match number {
    Ok(_n) => "thank you for entering a number",
    Err(_) => "please enter an number",
};
println!("Regarding {:?}, {}", number, msg);
// Regarding Ok(123), thank you for entering a number
```

You can also use a range of combinators to work with results. The common ones are:

| Function        | Purpose           |
| ------------- |-------------|
| `map`         |Applies a function if the result is an `Ok`.
| `and_then`    |Sequence this result with another result (`flatMap` or `bind` in other languages).
| `map_err`     |Apply a function if the result is an `Err` (`leftMap` in Scala's Cats library).
| `or`          |Combine two results, giving you the first `Ok` (if any).
| `or_else`     |Turn an `Err` into an `Ok` with a value you specify.
| `unwrap_or`   |The content of an `Ok`, or the default value you specify.

For example, here's `map`:

```
let plus_1 = "99".parse::<u16>().map(|n| n + 1);
println!("{:?}", plus_1);
// Ok(100)
```

# Combining errors of the same type

The `and_then` combinator can combine two results ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=ee3c43a872a45781b24baa278d293799)):

```
use std::num::ParseIntError;

fn add(input1: &str, input2: &str) -> Result<u16, ParseIntError> {
    input1.parse::<u16>()
        .and_then(|a| input2.parse::<u16>().map(|b| a + b))
}

fn main() {
    println!("Success result:  {:?}", add("1", "2"));
    println!("Example failure: {:?}", add("1", "boom"));
}
```

The output is:

```
Success result:  Ok(3)
Example failure: Err(ParseIntError { kind: InvalidDigit })
```

I find that straightforward, if a little tricky to read.
Rust also has a fast-fail short cut for this, via the `?` operator ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=c3cbae2676130a357363133368a06655)):

```
fn add(input1: &str, input2: &str) -> Result<u16, ParseIntError> {
    let a: u16 = input1.parse()?;
    let b: u16 = input2.parse()?;
    Ok(a + b)
}
```

The `?` at the end of the assignment to `a` and `b` is extracting the value from the `Result`.
It can only do this for `Ok` values. 
`Err` values are immediately returned from `add`.
`?` is only applicable in the context of a `Result` return type (or `Option`, but that's not our focus here).

The `?` operator is almost equivalent to the `and_then` version or pattern matching, with a twist we'll see later.

Notice that in the code above, all the types line up because we're always working with the same error type: `ParseIntError`.
That's not always the case.

# Combining errors of differing types

When there's no relationship between different types of errors, we need to work harder to combine results. Suppose we want to parse a URL, then append a page number to it. The `url` crate provides a parser:

```
pub fn parse(input: &str) -> Result<Url, ParseError>
```

Let's combine this with our number parsing. I've been explicit about the types of `parsed_url` and `parsed_int` to make the problem clearer hopefully ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=37bd9c32fd7aad1283a80fa7c25c5dc9)):

```
// WILL NOT COMPILE - NOT EXPECTED TO COMPILE

fn page_link(input_url: &str, input_page: &str) -> Result<String, ParseError> {
    let parsed_url: Result<Url, ParseError> = Url::parse(input_url);
    let parsed_int: Result<u16, ParseIntError> = input_page.parse::<u16>();

    parsed_url.and_then(|url| parsed_int.map(|page| format!("{}?page={}", url, page)))
}
```

The compiler tells us the problem is in the `map`: it expected  `ParseError` but found struct `ParseIntError`. 
The problem is with me. I've said the type of `page_link` is a string or a `ParseError`, but what could I put in there to cater for both a URL parse error and a number parse error?

One solution is to pick one of the errors as a reasonable choice for `page_link` ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=6962e20909fd844b00bb249da1d1b9da)):

```
use url;
use url::{ParseError, Url};

fn page_link(input_url: &str, input_page: &str) -> Result<String, ParseError> {
    let parsed_url: Result<Url, ParseError> = Url::parse(input_url);
    
    let parsed_int: Result<u16, ParseError> = 
        input_page.parse::<u16>()
        .map_err(|_err| ParseError::InvalidDomainCharacter);

    parsed_url.and_then(|url| parsed_int.map(|page| format!("{}?page={}", url, page)))
}
```

I've picked the url crate's error for my function, and mapped number parsing into one of the error variations. I guess `InvalidDomainCharacter` is kind of sort of like a parsing error, but it's a kludge.

It's useful to be able to do this, but we can do better.
 
# Application errors

I'm more likely to introduce my own application (or module) specific errors. Perhaps I'd define a `Mishap` enumeration with a couple of values:

```
#[derive(Debug)]
enum Mishap {
    BadUrl,
    BadPageNum,
}
```

Of course, I could have had just one catch-all error for parsing, or I could have included some context in my errors, but for now, I'm just marking two different kinds of errors.

With that, I can then have my function return a `Mishap`, and `map_err` lower-level errors into my error types ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=9bec98ba2814f2c546bd8f581bb2ad83)):

```
use Mishap::{BadPageNum, BadUrl};

fn page_link(input_url: &str, input_page: &str) -> Result<String, Mishap> {
    let parsed_url: Result<Url, Mishap> = 
        Url::parse(input_url).map_err(|_err| BadUrl);
    
    let parsed_int: Result<u16, Mishap> = 
        input_page.parse().map_err(|_err| BadPageNum);

    parsed_url.and_then(|url| parsed_int.map(|page| format!("{}?page={}", url, page)))
}
```

There's a consistent pattern here: always `map_err` into `Mishap`,
then sequence a computation at the end of the function.

This works as you might expect:

```
fn main() {
    println!("Success: {:?}", page_link("https://example.org", "5"));
    println!("Failure: {:?}", page_link("https://example.org", "five"));
}

Outputs:

Success: Ok("https://example.org/?page=5")
Failure: Err(BadPageNum)
```

Now that the error types all lineup, I can also use the `?` short-cut too ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=66b54b0a8a1df6ea6005137949bc38fe)):

```
fn page_link(input_url: &str, input_page: &str) -> Result<String, Mishap> {
    let url: Url = 
        Url::parse(input_url).map_err(|_err| BadUrl)?;
    
    let page: u16  = 
        input_page.parse().map_err(|_err| BadPageNum)?;

    Ok(format!("{}?page={}", url, page))
}
```

I think that's a resaonably good way to get a readable function.

# From

Inserting `map_err` for one-off cases is fine, but it is tedious if you're doing this multiple times. Hopefully, you can isolate code to return one error type, and `map_err` it once. But if it is common, there's a type class called `From` that eases the way.

A simplified version of `From` is:

```
pub trait From<T> {
    fn from(t: T) -> Self;
}
```

`From` captures a conversion from one type, `T`, to the implementing trait  (`Self`). It's not specific to error handling but helps us because `?` calls `from` on errors, inferring the type we're going to from the function signature.

The upshot is that we can implement a conversion from specific errors into our application error ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=1758ceaf3038a7b4e801e5c28f9be35a)):

```
impl From<ParseIntError> for Mishap {
    fn from(_err: ParseIntError) -> Mishap {
        BadPageNum
    }
}

impl From<ParseError> for Mishap {
    fn from(_err: ParseError) -> Mishap {
        BadUrl
    }
}
```

And then omit the `map_err` calls:

```
fn page_link(input_url: &str, input_page: &str) -> Result<String, Mishap> {
    let url: Url = Url::parse(input_url)?;
    let page: u16 = input_page.parse()?;
    Ok(format!("{}?page={}", url, page))
}
```

To be clear, what's happening here is the `?` operators are causing fast-fail out of `page_link`.
We can use `?` here because the enclosing type is a `Result`. 
It compiles because `?` invokes the `From::from` call to the inferred type (`Mishap`).

Without the `From` instances in place for `url::ParseError` and `ParseIntError`, 
the above function would not compile.


# In practice

In an application, I end up doing a little of all these options: a few `From`s when it makes sense and some `map_err` at other times. There are existing crates to help with this, such as automatically deriving `From` implementations for you. I've only started using these recently.

One I like the is a crate called `thiserror`, which I've used to wrap lower-level errors, and provide `From` implementations.
It looks like this in use:

```
use thiserror::Error;

#[derive(Error, Debug)]
pub enum Mishap {
    #[error(transparent)]
    Network(#[from] native_tls::Error),

    #[error(transparent)]
    Imap(#[from] imap::error::Error),

    #[error(transparent)]
    File(#[from] std::io::Error),

    #[error("Upload failed: {0}")]
    UploadRejected(http::status::StatusCode),
}
```

# The standard error trait

`thiserror` and other libraries also implement a standard Rust trait called `std::error::Error`. The intention is to capture some minimal error functionality. Functions like `source()` to give you lower-level errors, and `to_string()` for a message (via the `Display` trait, which is a kind of `Show` from other languages).

What that means is you don't need to care about the specific error. It is not something I lean towards, but when glueing together code at a `main`-level, I see the value of not caring at that point what the error is, other than the fact I have an error.

Here's how `std::error:Error` looks in the example I've been using ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=caaff685d2c03869d4a3709698417aaf)):

```
type Result<T> = std::result::Result<T, Box<dyn std::error::Error>>;

fn page_link(input_url: &str, input_page: &str) -> Result<String> {
    let url: Url = Url::parse(input_url)?;
    let page: u16 = input_page.parse::<u16>()?;
    Ok(format!("{}?page={}", url, page))
}
```

As the return type is getting long, I've introduced a type alias called `Result<T>`. The error type is the exciting part. Working from the inside:

- We're dealing with an error that implements the trait `std::error::Error`.
- There's some error at runtime that implements the trait, but we do not know at compile time what it is exactly. It is "dynamic", in the sense that there needs to be extra information carried around to know how to dynamically dispatch method calls.
- We also do not know the size of the error type. So we put it inside a `Box`, which is a pointer out to the heap.

I've found the `dyn` part easier to understand in comparison to the alternative. Consider:

```
fn known_at_compile_time<E>(err: E) -> String
where
    E: std::error::Error,
{
    err.description().to_string()
}
```

I'm more comfortable with code like this. We've written a generic function that takes any type `E` as an argument, with the constraint that there must be an implementation of `std::error::Error` for `E`.  It means at compile-time, we know the specific type for `E`. That's in contrast to the `dyn Error` which is stating something at runtime will implement the `Error` trait.

The result of using `std::error::Error` is pretty much the same:

```
fn main() {
    println!(
        "Success:  {:?}",
        page_link("https://example.org", "5")
    );
    // Success:  Ok("https://example.org/?page=5")
}

```

...but I can't exhaustively match on my specific application errors. I usually want to be able to do that, but not all the time.

Notice that we do not need to implement `From` because the url crate does this, as does the internal `ParseIntError`. It's a common practice for libraries to implement the `std::error::Error` trait.

# Summary

Most of the time I'll encode errors into a module (or application) specific enum. Then I'll either `map_err` or use `From` to adapt errors as I chain computations together.  Crates like `thiserror` reduce some of the boilerplate for this, and give a framework for describing errors.

# Resources

[rrqm]: https://doc.rust-lang.org/reference/expressions/operator-expr.html#the-question-mark-operator
[rr]: https://doc.rust-lang.org/reference/introduction.html
[ttor]: https://joshleeb.com/posts/rust-traits-and-trait-objects/
[book]: https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html
[cats]: https://typelevel.org/cats/datatypes/either.html#either-in-the-small-either-in-the-large
[thiserror]: https://github.com/dtolnay/thiserror
[snafu]: https://crates.io/crates/snafu
[anyhow]: https://github.com/dtolnay/anyhow
[reddit]: https://www.reddit.com/r/rust/comments/dfkwfo/announcing_thiserror_a_convenient_modern/
[gb]: https://books.google.co.uk/books?id=0Evsbi34TlcC&pg=PA255&dq=pop11+mishap&hl=en&sa=X&ved=0ahUKEwiEoLbM25DnAhWIYcAKHc7mDAIQ6AEISjAE#v=onepage&q=pop11%20mishap&f=false
[ti]: https://rust-lang.github.io/rustc-guide/type-inference.html#type-inference
[apiresult]: https://doc.rust-lang.org/std/result/enum.Result.html
[morf]: https://doc.rust-lang.org/book/ch10-01-syntax.html?#performance-of-code-using-generics
[to]: https://doc.rust-lang.org/book/ch17-02-trait-objects.html#trait-objects-perform-dynamic-dispatch

- [Recoverable Errors with `Result`][book] from the Rust book is the introduction to `Result`.
- The [API documentation for `Result`][apiresult] gives details on the methods and implementations available from the standard library.
- [The Question Mark Operatior][rrqm] from the [Reference Book][rr].
- The [Scala Cats documentation discusses `Either`][cats], and how to define application-level errors.
- I found Josh Leeb-du Toit's 2018 post on [Traits and Trait Objects in Rust][ttor] a great help.
- The [rustc guide discusses Rust type inference][ti].
- When comparing `dyn` to a known type a compile time, I was alluding to [monomorphization][morf] and [trait objects][to] using dynamic dispatch.
- I used [the thiserror crate][thiserror] in this post, but [snafu] looks good too. There's a good general [discussion at Reddit on error crates][reddit].
- In terms of ignoring errors to some degree, [anyhow] looks very tempting.
- I used `Mishap` as an enum name in this post. This is the name given to errors in POP-11 because [it causes less distress than the word Error][gb].

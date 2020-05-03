---
title: |
    Rust principles
author: Richard Dallaway
layout: post
---

I'm accumulating notes on the principles in Rust. These are the things I want to keep in easy reach to refresh my understanding.

<!-- break -->

# Contents
{:.no_toc}

1. Contents
{:toc}


## Sources

- [The Rust Programming Language](https://doc.rust-lang.org/stable/book/), Steve Klabnik and Carol Nichols, with contributions from the Rust Community.
- [Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/).
- [Rust in Motion](https://www.manning.com/livevideo/rust-in-motion), Carol Nichols and Jake Goulding, Manning liveVideo.
- [The Rust Reference](https://doc.rust-lang.org/reference/introduction.html).
- [Rust RFCs](https://rust-lang.github.io/rfcs/introduction.html).
- [The Rustonomicon](https://doc.rust-lang.org/nomicon/index.html).


## Ownership rules

> Each value in Rust has a variable that’s called its owner. There can only be one owner at a time. When the owner goes out of scope, the value will be dropped. [1]

It's more than memory management: it applies to sockets and locks, for example.

## Borrowing

> Most of the time, we'd like to access data without taking ownership over it. To accomplish this, Rust uses a borrowing mechanism. Instead of passing objects by value (`T`), objects can be passed by reference (`&T`). [3]

## References

Consider: `&foo` or `&mut foo`.

> These ampersands are references, and they allow you to refer to some value without taking ownership of it.

> At any given time, you can have either one mutable reference or any number of immutable references. [2]

Use `*` to dereference a reference [9].

## Implicit deref coercions

> Implementing the `Deref` trait allows you to customize the behavior of the dereference operator, `*` [6]

From [7]:

> _Deref coercion_ is a convenience that Rust performs on arguments to functions and methods. `Deref` coercion converts a reference to a type that implements `Deref` into a reference to a type that `Deref` can convert the original type into.

> `Deref` coercion happens automatically when we pass a reference to a particular type’s value as an argument to a function or method that doesn’t match the parameter type in the function or method definition.

> A sequence of calls to the `deref` method converts the type we provided into the type the parameter needs.

There are other coercions:

> They mostly exist to make Rust "just work" in more cases, and are largely harmless. [8]

## Lifetime elision

From my notes from _Rust in Motion_:

A lifetime is the time during which a value is at a particular memory location. Generic lifetimes are needed so the compiler can prove references are valid for some concrete lifetime.

The three lifetime elision rules mean you often do not need to explicitly state the lifetime. That is, we can omit lifetimes for common cases based on a type signature (not code body).

From [4]:

> The first rule is that each parameter that is a reference gets its own lifetime parameter [...]

> The second rule is if there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters [...]

> The third rule is if there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters.


## Argument dereferencing pattern

Patterns destructure values.

When pattern matching, `&x` is dereferencing (a mirror of the way a value is constructed).


## Match binding mode

> ...patterns operate in different binding modes in order to make it easier to bind references to values. When a reference value is matched by a non-reference pattern, it will be automatically treated as a `ref` or `ref mut` binding. [10]

> Default binding mode: this mode, either `move`, `ref`, or `ref mut`, is used to determine how to bind new pattern variables. When the compiler sees a variable binding not explicitly marked `ref`, `ref mut`, or `mut`, it uses the default binding mode to determine how the variable should be bound. Currently, the default binding mode is always `move`. Under this RFC, matching a reference with a non-reference pattern, would shift the default binding mode to `ref` or `ref mut`. [11]

For more examples, see Ivan Veselov’s post on [More advanced aspects of pattern matching in Rust](https://notes.iveselov.info/programming/refs-and-pattern-matching-in-rust).

## Dynamic dispatch

The `dyn` keyboard signifies a type that uses dynamic dispatch.

## Trait objects

Trait objects as parameters support dynamic dispatch.

> A trait object is an opaque value of another type that implements a set of traits. The set of traits is made up of an object safe base trait plus any number of auto traits. [17]

To be object-safe, a trait must:

- not return type `Self`; and
- have no generic type parameters.

See: [18] and [19].

## `Fn` and `fn`

- `fn` is a function pointer.

- `Fn` is a trait.

## Closures

A closure is:

>  a unique, anonymous type that cannot be written out. A closure type is approximately equivalent to a struct which contains the captured variables.

> The compiler prefers to capture a closed-over variable by immutable borrow, followed by unique immutable borrow [a closure-specific feature], by mutable borrow, and finally by move. It will pick the first choice of these that allows the closure to compile.

From [16].

## `move`

> If you want to force the closure to take ownership of the values it uses in the environment, you can use the `move` keyword before the parameter list. This technique is mostly useful when passing a closure to a new thread to move the data so it’s owned by the new thread. [12]

## Smart pointers

> Smart pointers, on the other hand, are data structures that not only act like a pointer but also have additional metadata and capabilities. [5]

Examples include `String`, `Vec<T>`. They are smart in that they own the memory they point at, and provide other capabilities such as knowing their length

Use `Box<T>` for values allocated on the heap:

- `Box<T>` is a a fixed size, and so can live on the stack.
- A good use case is for transferring ownership without copying the (potentially large) data pointed at.
- Another use case is for recursive data structures.

## Slices

> Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection. [13]

The type is: `&[T]`.

## Strings

Via [14]:

- `String` is a growable, owned, heap-allocated UTF-8 encoded string.

- `&String` is a reference to a `String`

- `str` is a string slice, the type of a `"string literal"`.

- `&str` is a borrowed string slice.

## The empty type

It is: `!`, called "Never" [15].


[1]: https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html#ownership-rules
[2]: https://doc.rust-lang.org/stable/book/ch04-02-references-and-borrowing.html
[3]: https://doc.rust-lang.org/stable/rust-by-example/scope/borrow.html
[4]: https://doc.rust-lang.org/stable/book/ch10-03-lifetime-syntax.html#lifetime-elision
[5]: https://doc.rust-lang.org/stable/book/ch15-00-smart-pointers.html
[6]: https://doc.rust-lang.org/stable/book/ch15-02-deref.html
[7]: https://doc.rust-lang.org/stable/book/ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods
[8]: https://doc.rust-lang.org/nomicon/coercions.html
[9]: https://doc.rust-lang.org/reference/expressions/operator-expr.html#the-dereference-operator
[10]: https://doc.rust-lang.org/reference/patterns.html#binding-modes
[11]: https://rust-lang.github.io/rfcs/2005-match-ergonomics.html
[12]: https://doc.rust-lang.org/stable/book/ch13-01-closures.html
[13]: https://doc.rust-lang.org/stable/book/ch04-03-slices.html
[14]: https://doc.rust-lang.org/stable/book/ch08-02-strings.html
[15]: https://doc.rust-lang.org/reference/types/never.html
[16]: https://doc.rust-lang.org/reference/types/closure.html
[17]: https://doc.rust-lang.org/reference/types/trait-object.html
[18]: https://doc.rust-lang.org/book/ch17-02-trait-objects.html#object-safety-is-required-for-trait-objects
[19]: https://doc.rust-lang.org/reference/items/traits.html#object-safety


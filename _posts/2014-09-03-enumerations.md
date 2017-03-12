---
layout: post
comments: true
permalink: /:title.html
title: 'Scala Enumerations'
author: Richard
date: 2014/09/03
---

Should you use Scala's built in `scala.Enumeration` class, or roll your own sealed class objects?  The answer depends on which you value more: having a single lightweight class, or better type safety.

If you want to skip the detail, just know to use `scala.Enumeration` if you need to limit the number of classes; otherwise prefer case objects or classes.

The rest of this post describes:

* the problem with Enumeration;
* the benefits of Enumeration; and
* the alternatives you can use.

## What's the problem with scala.Enumeration?

> "Enumerations must D.I.E."
([thread on scala-internals mailing list](https://groups.google.com/forum/#!topic/scala-internals/8RWkccSRBxQ))

Here are the main criticisms I've seen of `scala.Enumeration` (which I'll mostly just refer to as Enumeration from now on):

1. Enumerations have the same type after erasure.
2. There's no exhaustive matching check during compile.
3. They don't inter-operate with Java's `enum`.

If 3 is your main concern, just use Java's `enum`s. You can mix Java and Scala source files in a project.

Points 1 and 2 deserve examples, so you can decide if you care about this behaviour or not.

First, if you want method overloading with different Enumerations, that's not going to work out:

<script src="https://gist.github.com/d6y/8e770f7545e15c9aea13.js"></script>

Second, if you want to use Enumeration in pattern matching, you will not get a "match may not be exhaustive" warning:

<script src="https://gist.github.com/d6y/fb5406ec42f57c1c3eea.js"></script>

Note: the compiler is happy to accept that, even though the function can fail at runtime with a `scala.MatchError`.

You may or may not care about those points.

## What is an Enumeration?

For me, I'd expect an Enumeration to not have the problems listed above. This is fine, because Scala allows me to look at enumerations another way, which we will see in a moment.

To understand `scala.Enumeration`, look at it with a different view:

> I really think that enums should be lightweight. One class (or even two) per value is not acceptable. If you are willing to pay that sort of price, it's not too burdensome to just define the case objects directly. Enums fill a different niche: essentially as efficient as integer constants but safer and more convenient to define and to use. ([Martin on the scala-internals mailing list](https://groups.google.com/d/msg/scala-internals/8RWkccSRBxQ/U4y0XpRJfdQJ))

Under that view, you can see how Enumeration gives you plenty of positives:

- Values have an automatic identifier, which is a consecutive integer.
- Values have an nice name, which you don't have to declare yourself.
- Enumerations have an order (`Mon < Tue` is `true`).
- You can iterate the members.
- You do not end up with a class per member.

Except for the last point, you can have everything via a sealed case objects.  This is why we say it's the key deciding point on using `scala.Enumeration` or not.

## Sealed case objects

The alternative can be as simple or as involved as you need it to be for your problem.

As an example, let's say you're happy building up the list of values yourself, don't care about order or automatic naming:

<script src="https://gist.github.com/d6y/48cd9c518ae0c52f62ca.js"></script>

That's a minimal example. You can then build anything extra you need. To illustrate, here's the Oracle Java [Planets enum example](http://docs.oracle.com/javase/tutorial/java/javaOO/enum.html) converted to this style:

<script src="https://gist.github.com/d6y/5747ffc33f76f4217de9.js"></script>

This version has an ordered enumeration, and uses a macro to generate the set of all values.

<script src="https://gist.github.com/d6y/681283756e5be8c6542d.js"></script>

You don't need macros to achieve this. The set of enumeration values can be automatically populated in a parent class of your enumeration.  A good example of this
is the [Viktor Klang "DIY Enum"](https://gist.github.com/viktorklang/1057513).  I've also made the long Oracle Planets example of the DIY Enum available as [a gist](https://gist.github.com/d6y/376f1a4b178c343ff415), and all the code in this post is available in a [git repository](https://github.com/d6y/enumeration-examples).


## Conclusion

`scala.Enumeration` has a particular view of what it means to be an enumeration. Use it where it fits your view of an enumeration.

A sealed set of case objects is a good way to represent an enumeration.  You can make this as simple or as sophisticated as you need for your project.

(_Originally posted at [Underscore](http://underscoreconsulting.com/blog/posts/2014/09/03/enumerations.html)_).


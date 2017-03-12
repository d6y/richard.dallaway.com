---
layout: post
comments: true
permalink: /:title.html
title: 'Comparing closures in Java, Groovy and Scala'
author: Richard
date: 2008/06/14
alias: /comparing-closures-in-java-groovy-and-scala
tags:
---

On [Paul][]'s return from JavaOne this year, we spoke about [Neal Gafter][]'s Closures Cookbook talk. From what I understood, this was a
look at the [BGGA closures proposal][], and contained an example that
pushed hard on some of the tougher closure issues for Java. I thought it
might be fun to look at the Java example from the talk, and covert it to
[Scala][] and [Groovy][].

Why those languages? Because they are the three [JVM][] languages I'm most interested in. I suppose I could also have compared the closure
support in [Jython][], [JRuby][] or... well, there are [a few to choose from][], but this blog is going to be plenty long enough with just
three.

Let's start with the Java example that was given, remembering that this
is a proposed syntax, that may or may not make it to Java 7 or later. As
I understood the example it was this: imagine you want to add the
ability to time a block of code, and you wanted to do it in a way that
would look almost like a new keyword has been added to the language; and
you wanted to pass in a parameter to name what you were timing; and the
block you're timing returns a result, or might throw an exception. So,
quite an involved case.

Here's how the current Java proposal looks:

<script src="https://gist.github.com/3160052.js"> </script>

So we're timing a couple of operations, and we're doing this inside a
method, `f`, that returns an integer. The implementation would be....

<script src="https://gist.github.com/3160058.js"> </script>

As you can see, `time` takes an arbitrary text label and a block of
code, runs the block and tells you how long the block took to run and if
it succeeded or not.

That's the example that was given at JavaOne. Now for the same thing in
Groovy...

To make runnable code for Groovy (and for Scala), I had to decided to
time something. So I'm timing a block of code that randomly throws an
exception or returns something. And then timing a block of code that
just returns a number. In Groovy that would be:

<script src="https://gist.github.com/3160066.js"> </script>

An example of running the code:

    $ groovy time.groovy
    a 37116000 true
    b 69000 true
    7

    $ groovy time.groovy 
    a 39998000 false
    Caught: java.io.IOException: Boom
	 at time\$\_f\_closure1.doCall(time.groovy:21)
	 at time\$\_f\_closure1.doCall(time.groovy)
	 at time.time(time.groovy:6)
	 at time.f(time.groovy:18)
	 at time.run(time.groovy:31)
	 at time.main(time.groovy)

Note that the Java example is typed in that it uses a generic type, `R`,
for the return value which gives you some compile-time checks. That is,
when you run `time` and use the result, the compiler will enforce that
your declaration of the result has the same type as the return type of
the block you're timing.

Although [Groovy does support generics][], I've not used them here, and
as a result the Groovy example doesn't have that type-safety. I think
that's the way one would typically write Groovy code.

UPDATE: as was pointed out to me in the comments on the [Java Lobby version of this blog post][], this isn't the same as the Java version.
In the Java version a `return` in the closure returns out of the
enclosing block.

Now a look at the same code in Scala:


<script src="https://gist.github.com/3160096.js"> </script>


I'm still not using Scala day-to-day, so this might be a little awkward:
thank you to the London Scala User Group for helping me clean up my
syntax, but all the mistakes are mine.

This code has the same properties as the Java code (type safety via the
`R` generic type), but seems a little shorter and neater. Additionally,
the thing I like about the Scala code (and the Groovy code) is that the
languages return the value of the last statement in a block, and that
the syntax allows a clean `time("thing") { ... }` format.

One observation: I've used the `Integer` class, which is deprecated, in
order to be able to print out the class of the return type in the
function `f()`. Without the `:Integer` declaration I was getting weird
compile errors. As I said, my understanding of Scala and type inference
isn't there yet.

The output from running the code:

	a 913000 true
	the answer, 42 is of type class java.lang.String
	b 13000 true
	seven is of type class java.lang.Integer
	
	a 936000 false
	java.io.IOException: Boom
	 at Main$$anonfun$1.apply((virtual file):26)
	 at Main$$anonfun$1.apply((virtual file):23)
	 at Main$.time$1((virtual file):8)
	 at Main$.f$1((virtual file):23)
	 at Main$.main((virtual file):44)
	 at Main.main((virtual file))
	// rest of stack trace removed

There's no conclusions here. It's just an exercise in comparing closures
code in three different languages. I've probably missed some of the
nuances of the Java example, but hey... it's a starting point.

  [Paul]: http://www.goulbourn.com
  [Neal Gafter]: http://gafter.blogspot.com/
  [BGGA closures proposal]: http://www.javac.info/
  [Scala]: http://www.scala-lang.org/
  [Groovy]: http://groovy.codehaus.org/
  [JVM]: http://en.wikipedia.org/wiki/Java_Virtual_Machine
  [Jython]: http://www.jython.org/Project/
  [JRuby]: http://jruby.codehaus.org/
  [a few to choose from]: http://en.wikipedia.org/wiki/JVM_Languages
  [Groovy does support generics]: http://groovy.codehaus.org/Generics
  [Java Lobby version of this blog post]: http://java.dzone.com/articles/comparing-closures-java-groovy

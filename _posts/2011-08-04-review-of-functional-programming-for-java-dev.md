---
layout: post
comments: true
permalink: /:title
title: 'Review of &#34;Functional Programming for Java Developers&#34;'
author: Richard
date: 2011/08/04
alias: /review-of-functional-programming-for-java-dev
tags:
---

<img src="http://akamaicovers.oreilly.com/images/0636920021667/cat.gif" width="180" height="236" />

*Dean Wampler, 2011, [Functional Programming for Java Developers][],
O'Reilly Media. 88 pages.*

**Summary**

This is the book for you if you code in Java, you've heard of functional
and want to know what the fuss is about, but you want it in terms of the
constructs of Java that you already know.  "This book explains why
functional programming has become an important tool for the challenges
of our time" and acknowledges that "much of the literature on functional
programming is difficult to understand for people who are new to it."  

In 88 pages there's a limit to what can be covered, and sure I have some
reservations, but I don't know of a better introduction for Java
developers who aren't comfortable looking at other languages.

**Details**

The first chapter addresses the "why functional?" question, which is
answered primarily in terms of bringing clarity to thinking, more reuse
and more concise code, and a better fit to "the unique challenges of our
time", such as "working with massive data sets and remaining agile".
 But you're not expected to jettison what you know, with later chapters
suggesting when mixing in OO is a great idea.

The detailed answer to the "why" question talks in terms of concurrency,
big data sets, modularity benefits, keeping code minimally sufficient,
reining in complexity—making the argument that functional programming
helps you in your job in all the areas, and these are areas you need to
be able to handle today.

In describing what functioning programming is (chapter 2), you get to
see some Java code simplified via the [Functional Java project][]
library.  In this chapter, and rolling into chapter 3, types and data
structures are introduced, via an implementation of Option (how to avoid
null) and List in Java.  This is all, I think, genuinely useful,
especially with the "tips" in the text and the exercises at the end of
the chapters.

**Reservations**

Tackling this subject in Java is [a big ask][], and around half way
through the book it starts to show.  When filter, fold and map are
introduced I suspect Java programmers drawn to this text may be
scratching their heads asking why they would ever want to write code
like this.  

A later chapter introduces pseudo code to illustrate concepts in a
Java-like syntax.  This isn't going to compile, and you have to ask
yourself if the text has drifted too far from Java. The author puts it
well when suggesting the next place to go is "writing real code",
pointing in the direction of a variety of languages that have anonymous
functions and higher-order functions... which is probably any language
except Java.

That said, the chapter on concurrency points to the Java [Akka implementation of Actors][], which looks like something you might want
to use straight away.  It's noted that the Actor model "isn't really a
functional approach to concurrency", but it's part of the culture.

**Conclusion**

A concise and personal look at functional programming from the Java
point of view, with some practical and compelling points about the
benefits on the large (concurrency) and small (bugs in a class) scale.
 If you're drawn to this area, you'll going to find this book valuable.
 But you know you're only delaying switching to another language ;-)

---

**Comments**

Aug 04, 2011: [deanwampler](https://twitter.com/deanwampler) said...

_Thanks for the thoughtful review. As you can see, it's tough to cover this subject in a short book and discussing ideas like pattern matching in Java is a challenge. If I inspire skeptics to take FP more seriously, my work will be done here. ;)_
 


  [Functional Programming for Java Developers]: http://oreilly.com/catalog/0636920021667
  [Functional Java project]: http://code.google.com/p/functionaljava/
  [a big ask]: http://idioms.thefreedictionary.com/a+big+ask
  [Akka implementation of Actors]: http://akka.io/

---
layout: post
comments: true
permalink: /:title.html
title: 'The Smileys in Scala'
author: Richard
date: 2009/05/17
alias: /the-smileys-in-scala
tags:
---

or: *a visual interpretation of some of the syntax of the Scala
programming language, with the aim of providing an aide memoire.*

The Happy Walrus     `: =>`
---------------------------

(Yeah, I needed to include the : to make this work).

"The [walrus][] prefers shallow shelf regions and forages on the sea
bottom." And like the walrus, in Scala the symbol for by name parameters
allows the shallow surface parameter to forage deep inside your code.

An example from [Beginning Scala][] of a method that keeps appending the
results of a block of code until the test is true:

    def bmap[T](test: => Boolean)(block: => T): List[T] = {
     val ret = new ListBuffer[T]
     while (test) ret += block
       ret.toList
     }

Here we see two walruses, `test` and `block`, neither of which are
evaluated as parameters until you dive into the body of the method.

The Scissors     `>:`
---------------------

Putting a lower bounds on a type is rather like cutting off the option
of passing a subclass. That is, anything to the left of the scissors
in...


    def doStuff[B >: T](p: B) = ...

... is snipped away if the type to the left is a subclass ("below") the
type on the right. I find it helps to mentally rotate 90 degrees
anti-clockwise to see the scissors cutting B if it's "below" T in the
[class hierarchy][].

Chapter 19 of [Programming in Scala][] has the details, as does 8.3 of
[Scala By Example (PDF)][].

Let's wield the scissors, by considering a manager who can't handle
detail: they can think about fruit and apples, but nothing more
specific...


    scala> case class Fruit()
    defined class Fruit

    scala> case class Apple() extends Fruit
    defined class Apple

    scala> case class Pippin() extends Apple
    defined class Pippin

    scala> class Manager[A >: Apple]
    defined class Manager

    scala> new Manager[Fruit]
    res2: Manager[Fruit] = Manager@3b2000a5

    scala> new Manager[Apple]
    res3: Manager[Apple] = Manager@276c9124

    scala> new Manager[Pippin]
    :11: error: type arguments [Pippin] do not conform to class Manager's type parameter bounds [A >: Apple]



The Fussy Bird     `<:`
-----------------------


When it comes to...


    A <: B 


...the fussy bird knows that the morsel on the left is the special,
tastier class when compared to the big old lump of class to the right.
In Scala it signifies an upper bound, that `A` must be the same or a
subtype (specialism) of `B`. And special means tasty, so no wonder she's
got her beak and eyes pointing left towards the tasty class.


    scala> case class Stuff()
    defined class Stuff

    scala> case class Food() extends Stuff
    defined class Food

    scala> case class TastyFood() extends Food
    defined class TastyFood

    scala> def peck[A <: Food](p: A) = println("yum")
    peck: [A <: Food](A)Unit

    scala> peck(Food())
    yum

    scala> peck(TastyFood())
    yum

    scala> peck(Stuff())
    :10: error: inferred type arguments [Stuff] do not conform to method peck's type parameter bounds [A <: Food] peck(Stuff())

The bird is so fussy she'll even be happy with `Nothing`.

So that's the three I've had to work to remember. I hope I've got that
far enough in the direction of correct to be useful. Someone will have
to sort out stories for `<%` (view bounds) and the rest of the syntax
you might see such as `/:`.


  [walrus]: http://en.wikipedia.org/wiki/Walrus
  [Beginning Scala]: http://www.apress.com/book/view/9781430219897
  [class hierarchy]: http://www.scala-lang.org/node/128
  [Programming in Scala]: http://www.artima.com/shop/programming_in_scala
  [Scala By Example (PDF)]: http://www.scala-lang.org/sites/default/files/linuxsoft_archives/docu/files/ScalaByExample.pdf

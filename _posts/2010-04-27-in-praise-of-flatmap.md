---
layout: post
comments: true
permalink: /:title.html
title: 'In Praise of flatMap'
author: Richard
date: 2010/04/27
alias: /in-praise-of-flatmap
tags:
---

In Scala the `flatMap` operation is astonishingly useful in everyday
programming. The texts on Scala that I've read only hint at how useful
it is: [Beginning Scala][] probably does the best job, and even then you
could miss it.  So I'll give it a go...

Map is an operation you probably know pretty well.  It takes a function
of type `A ⇒ B`, and applies it to everything in your collection of `A`s
so you end up with a collection of `B`s:

    Welcome to Scala version 2.7.7.final (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_17). 
    Type in expressions to have them evaluated. Type :help for more information.   
    scala> List(1,2,3) map { num => "I like to count: "+num } 
    res0: List[java.lang.String] = List(I like to count: 1, I like to count: 2, I like to count: 3)

In this case we're mapping `Int ⇒ String`, and given a list of `Int`s
we get back a list of `String`s. Simple.

The signature for `flatMap` is different.  It expects a function of type
`A ⇒ Iterable[B]`. In the above example, if we started with our list of
`Int`s, we'd need to supply a function for `flatMap` that, when called
on each `Int`, returned a `List[String]`.  Which just goes to show that
I picked a poor example, as it's hard to see how that'd be useful.  But
where `flatMap` comes into its own is with real code especially when you use
`Option`.  

The thing about `Option` is that you can think of it like a `List` that
happens to contain either zero items or one item.  When you think of it
like that it makes sense for `Option` to behave as a thing that can be
iterated over. Sure, you'll only get at most one iteration, but the
principle applies, and indeed `Option` does implement `filter`,
`foreach`, and `map`.  This means, via mechanisms I'm a little hazy on,
you can happily call `flatMap` with a function that
maps `A ⇒ Option[B]` because of this property of `Option` behaving like
an interable thing.

Combine this with the fact that `flatMap` will drop entries that are
empty (which for `Option` means `None`) and you have a way to get at all
the useful content of a `List`.  Here's a cut down example of processing
some XML, where we're going to extract a list of numbers, but only if we
can make sense of the numbers.

    scala> import scala.xml._ 
    import scala.xml._   
    
    scala> val xml = <outer><ul><li>1</li><li>2</li><li/><li></li><li>4</li></ul></outer> 
    xml: scala.xml.Elem = <outer><ul><li>1</li><li>2</li><li></li><li></li><li>4</li></ul></outer>   
    
    scala> def f(li: NodeSeq) : Option[Int] = {     
    | val content = li.text     
    | if (content != "")     
    |    Some(content.toInt)     
    | else     
    |    None     
    | } 
    f: (scala.xml.NodeSeq)Option[Int]   
    
    scala> xml \\ "ul" \ "li" flatMap f  
    res1: Seq[Int] = ArrayBufferRO(1, 2, 4)

We took some XML, and for all the `<ul>` tags, for all the `<li>`
tags, we applied the `f` function via `flatMap`.  This function, `f`,
returns either `Some[Int]` (useful) or `None` (not useful), and
`flatMap` glues all the useful values together for us.  

To me that's a powerful tool for getting stuff done without a lot of
code.

Aside:

`flatMap` also works well with pattern matching, especially when you're
not expected to catch all cases.  The above could have been written
as...

    xml \\ "ul" \ "li" flatMap { case <li>{num @ _*}</li> => num } map { _.text.toInt }

...separating the "finding of useful stuff" step from the "turning it
into an Int" step.  

 

  [Beginning Scala]: http://apress.com/book/view/9781430219897

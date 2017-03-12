---
layout: post
comments: true
permalink: /:title.html
title: 'The Scala REPL is great for Java developers too'
author: Richard
date: 2009/09/17
alias: /the-scala-repl-is-great-for-java-developers-t
tags:
---

This post is for Java developers who have heard about Scala but,
although it might sound interesting, are getting on with what they need
to do in Java thankyouverymuch.

If that's you, it's still worth [installing Scala][] because the
command-line tool makes noodling with Java a delight. The tool in
question is the REPL ([read-eval-print-loop][], a language shell).

Two quick examples...

Let's say there's some API you're going to use, but you just can't quite
remember the format of the return result. The REPL is a great way to
quickly find out what you actually get. Here's what I found out looking
for the list of all `TimeZone`s:


    $ scala
    Welcome to Scala version 2.7.5.final [...]
    Type in expressions to have them evaluated. Type :help for more information.

    scala> import java.util.TimeZone
    import java.util.TimeZone

    scala> TimeZone.getAvailableIDs()
    res2: Array[java.lang.String] = Array(Etc/GMT+12, Etc/GMT+11, MIT, Pacific/Apia, Pacific/Midway, Pacific/Niue, Pacific/Pago\_Pago, Pacific/Samoa, US/Samoa, America/Adak, America/Atka, Etc/GMT+10, HST, Pacific/Fakaofo, Pacific/Honolulu, Pacific/Johnston, Pacific/Rarotonga, Pacific/Tahiti, SystemV/HST10, US/Aleutian, US/Hawaii, Pacific/Marquesas, AST, America/Anchorage, America/Juneau, A...

Most of the developers I know might do a double take at the results
syntax but wouldn't have a problem reading that
`TimeZone.getAvailabeIDs()` gives me back an array of strings like
"Pacific/Tahiti". No Scala knowledge required to make use of that tool
(ok, I left the line-ending semi-colons out, but you can put them in if
you like).

Second example. Quick! Answer this: what does `String.split` return if
there are no matches to the pattern? Not sure? Try it and see:

    scala> "Pacific/Tahiti".split("/")
    res3: Array[java.lang.String] = Array(Pacific, Tahiti)

    scala> "wibble".split("/") 
    res4: Array[java.lang.String] = Array(wibble)

Hope that's useful. Don't miss that you have command history editing
(arrow keys on my keyboard).

If you want to do more take a look at [First Steps to Scala][].

  [installing Scala]: http://www.scala-lang.org/downloads
  [read-eval-print-loop]: http://en.wikipedia.org/wiki/Read-eval-print_loop
  [First Steps to Scala]: http://www.artima.com/scalazine/articles/steps.html

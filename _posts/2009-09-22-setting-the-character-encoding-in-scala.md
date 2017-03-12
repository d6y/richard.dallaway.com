---
layout: post
comments: true
permalink: /:title.html
title: 'Setting the character encoding in Scala'
author: Richard
date: 2009/09/22
alias: /setting-the-character-encoding-in-scala
tags:
---

The only reliable way we've found for setting the default character
encoding for Scala is to set `$JAVA_OPTS` before running your
application:

    $ JAVA_OPTS="-Dfile.encoding=utf8" scala 
    Welcome to Scala version 2.7.5.final [...] 
    Type in expressions to have them evaluated. Type :help for more information.  
    scala> val x = "garçon"
    x: java.lang.String = garçon

Just trying to set `scala -Dfile.encoding=utf8` doesn't seem to do it.
Of course, you may not need this if your OS defaults to a sensible
character encoding. I'm lumbered with something called "MacRoman"...

You'll also want to make sure your terminal is set to UTF-8 encoding,
which on the Mac is Terminal -\> Preferences -\> Settings -\> Advanced
-\> International.


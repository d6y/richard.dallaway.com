---
layout: post
comments: true
permalink: /:title
title: 'Using an existing Scala + Maven project in Eclipse'
author: Richard
date: 2009/07/23
alias: /using-an-existing-scala-maven-project-in-ecli
tags:
---

In case you're having problems using the [Scala IDE for Eclipse][] with
Maven projects, here's what I do:

-   Generate the Eclipse project files from the shell:
    `mvn eclipse:eclipse`. If you already have Eclipse .project files,
    you'll have to remove them first or `mvn eclipse:clean`.
-   In Eclipse, do a File -\> Import... and select "General -\> Existing
    Projects into Workspace", navigate to my maven project and select
    it.
-   This gives you a Scala project, but you'll need to enable Maven:
    right click on the root of the project in the Package Explorer in
    Eclipse, select Maven menu and enable.


The plugins I have look like this:

<a href="https://www.flickr.com/photos/d6y/16150791926" title="4ff9dba17c0b4-11219728-0-media_httpfarm4static_rflEE by Richard Dallaway, on Flickr"><img src="https://farm9.staticflickr.com/8579/16150791926_b4dd7e0e33_o.jpg" width="500" height="330" alt="4ff9dba17c0b4-11219728-0-media_httpfarm4static_rflEE"></a>

...and it all works very well. The Maven plugin is the [m2 one][].

Troubleshooting: If that doesn't work first time for you: Project -\>
Clean... is your friend. If you're still having problems, a Refresh and
Close Project followed by an Open project is good. In the worst case,
close the project, exit Eclipse, remove the .metadata folder (at your
own risk), and restart.

You might also want to check out the [Scala IDE for Eclipse wiki
pages][] for how others use Maven.

And for the record, I'm using a Mac (64bit, 10.5.7), with Maven 2.2.0
configured in Eclipse preferences to be an External installation (not
the Embedded, but I don't know if that makes a difference or not), JDK
1.5, using Eclipse classic 3.4.2.


  [Scala IDE for Eclipse]: http://www.scala-lang.org/node/94
  [m2 one]: http://m2eclipse.codehaus.org/index.html
  [Scala IDE for Eclipse wiki pages]: http://lampsvn.epfl.ch/trac/scala/wiki/ScalaIDEForEclipse

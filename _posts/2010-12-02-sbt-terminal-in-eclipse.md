---
layout: post
comments: true
permalink: /:title.html
title: 'Running SBT (or any shell command) from Eclipse'
author: Richard
date: 2010/12/02
alias: /sbt-terminal-in-eclipse
tags:
---

<img src="/img/posts/flkexport2018/16150789236_fac6330d36_o.png" width="576" height="576" alt="4ff9dbd6dbe3b-35137865-0-0_Install">

Mostly when I'm writing code I'm in the [Scala IDE for Eclipse][], with
a separate terminal window containing SBT running `~test-quick` or
`jetty-run`. But flipping back and forth between terminal and Eclipse is
non-optimal, so I went digging for Eclipse + SBT integrations.

There are a few [mentioned on the SBT site][], but I realized all I
needed was a shell prompt as a view in Eclipse and I'd be happy.  And of
course it exists already as "Target Management Terminal", part of the
Galileo update site.

1.  Eclipse \> Install New Software....
2.  From "Work with:" select Galileo (or whatever's appropriate for your
    version of Eclipse).
3.  Type "terminal" in the search box, and select Target Management
    Terminal.
4.  Go through the download, restart process.
5.  Eclipse \> Preferences and search for Terminal if you want to invert
    the colours (I did).
6.  Window \> Show View \> Other and select Terminal to show the
    Terminal view in Eclipse.
7.  Click the green connect button and set up the type of terminal you
    want (ssh to localhost in my case, and I'm prompted for a password).
8.  Login, you have a shell, do what you want, such as cd into your
    project and run SBT.

Probably totally obvious, but I missed this trick of having access to a
shell from Eclipse.

  [Scala IDE for Eclipse]: http://www.scala-ide.org/
  [mentioned on the SBT site]: http://code.google.com/p/simple-build-tool/wiki/IntegrationSupport

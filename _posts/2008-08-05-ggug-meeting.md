---
layout: post
comments: true
permalink: /:title
title: 'GGUG Meeting'
author: Richard
date: 2008/08/05
alias: /ggug-meeting
tags:
---

I have a love/hate relationship with [Groovy][]. I do find it
wonderfully productive for hammering out a quick script to do something,
but I feel uneasy attempting anything larger without my crutches of
static analysis (strict shouty compilers and [Find Bugs][]) and knowing
there are great profilers if I need them. Yet, when returning to Java
I'm left thinking "why do I have to write so much code?".

You'll understand, then, why I find Scala so appealing.

Still, I do use Groovy, and mostly I use Groovy in the context of
[Grails][], where I enter small snippets of Groovy to get the job done.
Even here, though, I rely on an IDE to help and the only really usable
one is IntelliJ where they have done a great job on supporting Grails.
Having said that, the [Grails Podcast][] have mentioned that there's
been some good Grails work in Netbeans 6.5. Don't get me wrong: Netbeans
is a great tool, but in some respects it always seems to be "jam
tomorrow"....

To keep up-to-date with the Grails world, I headed up to the
[Skillsmatter][] for the [Groovy and Grails User Group Meeting][]. It
was good to see who's using Grails and how people are using Grails, to
chat with them at the pub, but also it was handy to hear [Graeme
Rocher][] give a "state of grails" talk. And there's some great stuff in
the pipeline for 1.1:

-   plugins are going to be installed once on your machine and only the
meta data will be written to your project, and the plugin system
will resolve dependencies for you;
-   the plugin system is going to be faster at finding and listing
plugins;
-   plugins can be scoped (e.g., this plugin just for testing, not
deployment);
-   improved Maven support (the Grails POMs have been published already,
I believe);
-   decoupling of components out of Grails, including GORM and GSP;
-   better Java integration, such as for JPA, JSP, portlets; and
-   OSGi support after 1.1.


And I suspect there will be hundreds of smaller changes too. All this is
schedule for Dec 08/ Jan 09, which is the same timeframe as the two new
Grails books: [Grails in Action][] and the 2nd edition of [The
Definitive Guide to Grails][] (I would not recommend buying the 1st
edition now, as it was based on Grails 0.3). As I'm mentioning books
I'll just say that I found [Programming Groovy: Dynamic Productivity for
the Java Developer][] to be pretty useful.

The event was recorded, and it looks like it will appear on the
[Skillsmatter Java podcast page][].

  [Groovy]: http://groovy.codehaus.org/
  [Find Bugs]: http://findbugs.sourceforge.net/
  [Grails]: http://grails.org/Home
  [Grails Podcast]: http://hansamann.podspot.de/
  [Skillsmatter]: http://skillsmatter.com/
  [Groovy and Grails User Group Meeting]: http://upcoming.yahoo.com/event/923449/
  [Graeme Rocher]: http://graemerocher.blogspot.com/
  [Grails in Action]: http://www.manning.com/gsmith/
  [The Definitive Guide to Grails]: http://www.apress.com/book/view/1590597583
  [Programming Groovy: Dynamic Productivity for the Java Developer]: http://www.pragprog.com/titles/vslg/programming-groovy
  [Skillsmatter Java podcast page]: http://skillsmatter.com/podcast/java-jee/grails


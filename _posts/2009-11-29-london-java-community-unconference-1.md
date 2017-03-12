---
layout: post
comments: true
permalink: /:title.html
title: 'London Java Community Unconference 1'
author: Richard
date: 2009/11/29
alias: /london-java-community-unconference-1
tags:
---

<a href="https://www.flickr.com/photos/d6y/15990532579" title="4ff9db9fbb57b-11219720-0-media_httpfarm3static_ChEED by Richard Dallaway, on Flickr"><img src="https://farm8.staticflickr.com/7560/15990532579_502e1457c8_o.jpg" width="500" height="375" alt="4ff9db9fbb57b-11219720-0-media_httpfarm3static_ChEED"></a>

Congratulations to the organizers of the [London Java Community][]: the
first unconference was a success.

IBM kindly hosted the event at their [Southbank building][], and it's a
great location for an unconference. It has a set of rooms that are just
the right size, but also has a central mingling point where you can meet
and chat with people. Possibly for the first time at a conference for me
the gaps between the sessions were as useful as the sessions themselves.

I ran a session to discuss what's stopping anyone from using Scala,
especially in existing Java projects. Kind of a negative title in some
ways, but the point is that I don't see any technical reasons not too as
the tool chain is there. I was interested to hear other's experiences.
[The slides from my session][] are online, but they were really just put
up to help kick start a discussion between the 19 people who were in the
room.

It tuns out there's a detailed Google Wave covering this and other
sessions, but I'll list the main points that were discussed here:

-   Learning: there seemed to be a reluctance to use Scala until you know "the Scala way" or understand functional styles. It does seem fair to say you'll get more benefit from adopting more of Scala, but I feel that you need to at least start using Scala to understand how it can improve what you do. The point was made that you can start using Scala in a Java-like way (there's absolutely nothing wrong with that), and get some benefits today.
-   What are the benefits? Aside from less code, code that is easier to understand (IMHO), increased developer joy or passion, I pointed to the previous session which covered The Bug of the Month which was a hairy threading-related issue: if you're doing anything with threads, you've probably got it wrong. The actors library in Scala can simplify concurrency.
-   Commercial support: There's no named organization backing Scala. So where does the warm Sun, IBM, Oracle feeling come from for management?
-   What areas is Scala best applied to? That is, what areas of Java would you prefer to use Scala for? The honest answer is all areas. There's no place where you'd prefer Java over Scala.
-   Selling to management: why take the risk? I think it's a case of why miss the opportunity, if you can deliver more reliable results faster, and keep developers happy and engaged.

I suggested—and it's not an original idea—that unit testing might be a
place to start as you can improve tests and gain experience. I put an
example of that in my slides that I ripped out of some production code
just before the presentation.

There were requests for more Scala resources (damn...perhaps I should
have done an intro to Scala session too) so here are the resources I've
found useful:

-   [scala-lang.org][] - for downloads of Scala and a list of the published books.
-   [http://daily-scala.blogspot.com/][] - short examples of idiomatic Scala.
-   [Scala Users mailing lis][], where you can ask questions.

In the pub afterwards, there was a Scala corner where the London Scala
User Group was reinvigorated. The [lsug.org][] has been recovered and
[Skillsmatter][] have kindly offered to host a monthly event, probably
on a Wednesday, starting in Jan 2010. Keep an eye on the LJC mailing
list for news.

You'll find [my photos of the event][] and the hash tag on
Twitter is [#ljcuc1][].


  [London Java Community]: http://www.meetup.com/Londonjavacommunity/
  [Southbank building]: http://www-304.ibm.com/jct01005c/isv/spc/southbank.html
  [The slides from my session]: https://docs.google.com/fileview?id=0B0EWEdbeorKeYWZhYzg4MGEtMGY2YS00NzVkLWIwOTYtMzFkMzA5YTY0Zjg5&hl=en
  [scala-lang.org]: http://www.scala-lang.org/
  [http://daily-scala.blogspot.com/]: daily-scala.blogspot.com
  [Scala Users mailing lis]: http://www.scala-lang.org/node/199#scala-user%20
  [lsug.org]: http://lsug.org
  [Skillsmatter]: http://skillsmatter.com/go/java-jee
  [my photos of the event]: https://www.flickr.com/search/?text=ljcuc1&sort=relevance&user_id=83551313%40N00
  [#ljcuc1]: http://twitter.com/#search?q=ljcuc1


---
layout: post
comments: true
permalink: /:title
title: 'Scala & Wicket London Meet Up'
author: Richard
date: 2009/02/05
alias: /scala-and-wicket-london-meet-up
tags:
---

<img src="http://awesomeness.openphoto.me/custom/201207/880058-11219734-0-media_httpfarm4static_fAyyb_870x550.jpg" width="480"></img>

I know next to nothing about the [Wicket][] web framework, but I was
intrigued by the [jWeekend][] [London Wicket User's Group][] meet up
last night. The topic was "Scala and Wicket".

As far as Scala and web frameworks go, the main name is [Lift][]. The
problem with Lift... No, let me re-phrase that because it's not a
problem with Lift. The great thing about Lift is that it's made for
Scala, so it's going a great fit to the language. Learning Lift and
Scala at the same time should mean you're bouncing the framework
learning and the language learning off each other, which is going to
teach you a lot.

Then again... if you're learning the language and trying to get your
head around a framework, it seems to make the goal of "doing something
useful with Scala" just that little bit further away. So how about this:
Scala and Java work well together, so why not use a web framework you
already know, but just use Scala instead of Java? Start gently, then,
when ready, take a look a Lift.

I don't know which approach is best, but it seems there might be
something in the gently-gently approach. There is one other compelling
reason for looking at a existing (legacy? :-) web framework: you can dig
into the publications on the topic, such as [Wicket in Action][], or
[Programming Struts][], or [Stripes][Programming Struts] etc. (Obviously
this situation will change for Lift: I've already expressed my
disappointment that none of the big publishers are looking at the
practical aspects of Scala, but there is the start of [a creative commons text][]).

Sure, you're not necessarily going to learn Scala idioms from a
non-Scala framework, and you're going to run into head-scratching
issues, but it seems somehow more manageable to at least try it. More so
if you're inserting Scala into an existing project.

Of course, the whole argument falls apart for me in the case of this
event, as I don't know Wicket :-) But I've tried Scala in a trivial way
with a large existing Struts 1 application, and it was surprisingly
painless.

But back to the event and the talks:

-   [Daan van Etten][] gave a lovely "Basic Introduction to Scala With Wicket". The [slides, handouts and code are available][]. One download and two commands to get the example app up and running was pleasing.
   

-   [Dean Phersson-Chapman][] spoke about his "Experiences Converting an Existing Wicket Application To Scala". It seems there are some serialization issues between Wicket and Scala 2.7.3 which are being fixed.

-   [Jan Kriesten][] showed examples of "Real World Scala and Wicket". If you'd not seen Scala before, this was probably pretty scary stuff in places. Jan clearly knows his Scala and his Wicket very well.

-   Finally, [Alastair Maw][] spoke about the evils of abstraction, which had some fine points about when to avoid it (mostly) and when to embrace it (rarely).

It looks like slides appear over at [londonwicket.org][London Wicket User's Group].


  [Wicket]: http://wicket.apache.org/
  [jWeekend]: http://jweekend.com/
  [London Wicket User's Group]: http://londonwicket.org/
  [Lift]: http://liftweb.net/
  [Wicket in Action]: http://www.manning.com/dashorst/
  [Programming Struts]: http://oreilly.com/catalog/9780596006518/
  [a creative commons text]: http://github.com/tjweir/liftbook/tree/master
  [Daan van Etten]: http://stuq.nl
  [slides, handouts and code are available]: http://stuq.nl/weblog/2009-02-04/download-the-basic-and-wicket-scala-talk-materials
  [Dean Phersson-Chapman]: http://www.imdplc.com/
  [Jan Kriesten]: http://www.footprint.de/fcc/
  [Alastair Maw]: http://herebebeasties.com/

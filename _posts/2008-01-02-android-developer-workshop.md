---
layout: post
comments: true
permalink: /:title.html
title: 'Android Developer Workshop'
author: Richard
date: 2008/01/02
alias: /android-developer-workshop
tags:
---

<img src="/img/posts/flkexport2018/2117882768_0f32f36687_o.jpg" width="480" height="640" alt="At android thing in London">

Half-day workshops are great: you get away from distractions; get to ask
questions and get quick answers to cover off those things that are
bugging you; you're more-or-less forced to spend more time hands-on with
a technology; you get a sense of the buzz around something and which
bits are good and bad; and... it's only half a day gone if it doesn't
turn out that way. For those and other reasons I attended the Android
Developer Workshop at Google London on 17th December 2007, hosted by
[Dick Wall][]. It did a fine job of highlighting the current state of
Android and which areas are the important ones to focus on.

The first thing that's very clear: you can get a lot done in Android in
not a lot of code.

If you're a Mac user the other thing you notice is that you can develop
on a Mac without jumping through hoops. It's almost like Google are
treating the Mac as a first class citizen, and I can't tell you how
convenient and wonderful that is.

I've been playing around with some of the APIs and finding it all well
documented already, and quite fun. For my own reference, useful pages
include: the [optional APIs][] home page, the [glossary][], the [class index][], the [application lifecycle][], and the [activity lifecycle][].

It's new, so there are gaps in the documentation. For example, when
putting together an LBS application, you discover that the emulator
supports mock data sources, but you need to dig around the mailing list
to find out how to register them. Say you have a KML file you want to
use as a location provider called "brighton". Call the file "kml" then
load that into the emulator from your shell with:
`$ adb push kml /data/misc/location/brighton/kml` and then restart the
emulator.

There's a lot that's not done yet or not figured out. One nice idea is
that there may be support for an XMPP-like server push. That is, your
Android app can listen out for server-generated Jabber-ish messages and
do something. How will that work in practice across the networks? How
will mobile clients be looked up and discovered? We can guess at these
things, but we don't yet know the answers.

So, early days. For now we only have emulators, so we won't get much of
a feel for how this will all pan out until there are at least a couple
of handsets out there. But it's powerful, well thought-out stuff. If
you're thinking about this space, I'd say it's at least worth spending a
day working through the tutorial application.

The event was recorded so I'd expect the video to show up somewhere on
[YouTube][] one day.


  [Media\_httpfarm3static\_hxjsa]: ./images/11219754-0-media_httpfarm3static_hxJsA.jpg.scaled500.jpg
  [Dick Wall]: http://javaposse.com/
  [optional APIs]: http://code.google.com/android/toolbox/optional-apis.html
  [glossary]: http://code.google.com/android/reference/glossary.html
  [class index]: http://code.google.com/android/reference/classes.html
  [application lifecycle]: http://code.google.com/android/intro/lifecycle.html
  [activity lifecycle]: http://code.google.com/android/reference/android/app/Activity.html
  [YouTube]: http://www.youtube.com/AndroidDevelopers

---
layout: post
comments: true
permalink: /:title.html
title: 'Google Developer Day 2007'
author: Richard
date: 2007/06/19
alias: /google-developer-day-2007
tags:
---

I was in London for the [GD Day 2007][]. I sat in on some pretty
interesting sessions:

-   [Gears][] does indeed look impressive. I was on the edge of my seat
waiting to hear how they'd solved online-to-offline synchronization,
but they explicitly backed out of that one: "What we've chosen to do
for this initial release is not tackle the synchronization problem
[...] overtime maybe there will be something that works for the 80%
case, and if that happens I think it's going to come from the
community". I can understand that, and it's probably a great
opportunity for anyone who can solve that problem. Worth catching
[the video][]. I'll also add that because it's open source, I
wouldn't be surprised if the plugin was ported to mobile devices
(Safari, after all, is already supported, so how far away can a
mobile [WebKit][] version be?).
-   Google have opened up their geolocation API. I think this is
absolutely huge for the UK, but right now it returns an error
message for any UK address. I learned that (a) it's going to be OK,
it's just the post office dragging their feet over the legals; and
(b) if you need the data, there's a hack to get it out of Google
Local Search.
-   There was a presentation on porting a JSP site to GWT. The lesson
seems to be that you start from scratch, but I did learn you can use
Java to implement impressive Javascript behaviors (drag and drop,
for example,) without writing any Javascript.


Go [watch the videos][].


  [GD Day 2007]: http://code.google.com/events/developerday/uk-home.html
  [Gears]: http://code.google.com/apis/gears/
  [the video]: http://youtube.com/watch?v=HsODVUvgvdk
  [WebKit]: http://webkit.org/
  [watch the videos]: http://youtube.com/googledeveloperday

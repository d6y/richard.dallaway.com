---
layout: post
comments: true
permalink: /:title
title: 'Introducing ReminderThing.com'
author: Richard
date: 2011/10/24
alias: /introducing-reminderthingcom
tags:
---

<object style="margin-left: 40px; width:500px" height="375" width="500"> <param name="flashvars" value="offsite=true&amp;lang=en-us&amp;page_show_url=%2Fphotos%2Fd6y%2Fsets%2F72157627781926581%2F%2Fshow%2F&amp;page_show_back_url=%2Fphotos%2Fd6y%2Fsets%2F72157627781926581%2F&amp;set_id=72157627781926581&amp;jump_to=" /></param> <param name="movie" value="http://www.flickr.com/apps/slideshow/show.swf?v=71649" /></param> <param name="allowFullScreen" value="true" /></param><embed src="//www.flickr.com/apps/slideshow/show.swf?v=71649" allowFullScreen="true" type="application/x-shockwave-flash" height="375" flashvars="offsite=true&amp;lang=en-us&amp;page_show_url=%2Fphotos%2Fd6y%2Fsets%2F72157627781926581%2F%2Fshow%2F&amp;page_show_back_url=%2Fphotos%2Fd6y%2Fsets%2F72157627781926581%2F&amp;set_id=72157627781926581&amp;jump_to=" width="500"></embed></object>

We rarely get to talk about our Lift work, but I can talk about this one
because it's been a Sunday afternoon project for a while.  

I live in Google for my contacts and calendar, and wanted a quick
summary of upcoming birthdays and anniversaries.  I'm [not alone][] in
wanting something like this.  [Reminder Thing][] itches this scratch by
digging into my contacts and calendar each week, and emailing me a
single reminder of upcoming birthdays and anniversaries.  

That's it.  Obviously, if you live in Facebook, you don't need this, as
they already sensibly send out such an email.  But I've not yet, um,
fully embraced Facebook.

But this is mostly a tech blog, so here are the details. It's a Scala
app, running on the [Lift framework][].  It uses [SendGrid][] to send
email, and uses Google's own AuthSub and GDATA Java APIs to communicate
with Google. The UI is Twitter's [Bootstrap][].

We build using [SBT][] and deploy using [SBT Cloudbees Plugin][] to a
single CloudBees 128M cell.  I believe Cloudbees is currently using JDK
1.6, Tomcat 6 and Nginx, but I neither know for sure and nor do I
especially care—if it works, it's fast and I doesn't give me hassle, I'm
happy. 

*BTW, If you're in the Sydney area and want to know more about how we
deploy Scala apps, to CloudBees or AWS, go to [ScalaSyd on 26 Oct
2011,][] where [Jono][] is on a panel discussing Scala in production.*

I have two tech observations from writing this code:

First, firing off asynchronous events to Google is a delight with Scala
and Lift.  The page where we show spinners while fetching data from
Google makes use of the [Lift LazyLoad][] CSS tag (which means it
becomes concurrent without me making any code changes) and a little bit
of [Lift Wiring][] (8 lines) to make the "Continue" button change
colour.

The other comment is that Lift parameterised menus really help you be
productive: very worth getting to know.  In Reminder Thing the
unsubscribe link plus the ability to undo your unsubscribe uses
parameterised menus:

<script src="https://gist.github.com/1309016.js"> </script>

So that's Reminder Thing.  There are a couple of tweets on adding
birthdays and anniversaries  from [@rmndrthng][], which is also the
right Twitter account to use to let us know if you like the idea,
anything that doesn't work, anything we can do better.  

If you're a Google contacts or calendar user, do [give it a go][].

 

  [not alone]: http://www.google.com/support/forum/p/gmail/thread?tid=6e5008d077277e14&hl=en
  [Reminder Thing]: http://www.ReminderThing.com/
  [Lift framework]: http://liftweb.net/
  [SendGrid]: http://sendgrid.com/
  [Bootstrap]: http://twitter.github.com/bootstrap/
  [SBT]: https://github.com/harrah/xsbt/wiki
  [SBT Cloudbees Plugin]: https://github.com/timperrett/sbt-cloudbees-plugin
  [ScalaSyd on 26 Oct 2011,]: http://www.meetup.com/scalasyd/events/34058882/
  [Jono]: http://twitter.com/jonoabroad
  [Lift LazyLoad]: http://seventhings.liftweb.net/lazy
  [Lift Wiring]: http://seventhings.liftweb.net/wiring
  [@rmndrthng]: http://twitter.com/rmndrthng
  [give it a go]: http://www.ReminderThing.com

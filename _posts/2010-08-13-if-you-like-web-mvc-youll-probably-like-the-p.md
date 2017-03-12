---
layout: post
comments: true
permalink: /:title.html
title: 'If you like web MVC you&#39;ll probably like the Play web framework'
author: Richard
date: 2010/08/13
alias: /if-you-like-web-mvc-youll-probably-like-the-p
tags:
---

Play is a MVC, convention-based, stateless web framework for Java with growing support for Scala too.

It's not for me as I can't face going back to MVC and the kinds of
presentation languages they use. Having said that, if you like MVC, and
you're not already using Grails or Rails or a similar framework, I
strongly urge you to [look at Play][] as there's some nice technology in
there. 

Everything I know about Play comes from [Rustem Suniev][]'s talk for the
[London Scala User Group][] at Skilsmatter on Wednesday. The [slides and video are already on-line][] and they contain a really nice live-coding
demo of Play which gave me a good sense of what the framework is about.
 Nice work—and pizza courtesy of [autoquake.com][] (who [are hiring][]).

One comment I will make is that Play pushes it's stateless-ness
prominently. For many of us I suspect our default position is that
stateful=bad and stateless=good.  That sounds sane, but you probably do
need some state in your application, and you have to deal with it
somehow, or push the issue somewhere, which leaves me feeling that state
v. stateless thing is all a bit more complicated that we often think it
is.  It certainly does not automatically mean better scaling or
performance, but there are definite positives to it.  I'm glad to see
the Play community [discussing state][] and [exploring some nice ideas][].  Just don't assume a label of "stateless" solves all your
scaling problems—if you're lucky enough to have any :-)

 

  [look at Play]: http://www.playframework.org/
  [Rustem Suniev]: http://twitter.com/vigosun
  [London Scala User Group]: http://lsug.org/
  [slides and video are already on-line]: http://skillsmatter.com/podcast/scala/scala-podcast-intro-to-play/rl-890
  [autoquake.com]: http://www.autoquake.com/
  [are hiring]: http://www.autoquake.com/jobs
  [discussing state]: http://groups.google.com/group/play-framework/browse_thread/thread/ac9dd976cb284bad/fe7beddf7fabcc38?lnk=gst&q=stateful#fe7beddf7fabcc38%20%20
  [exploring some nice ideas]: http://groups.google.com/group/play-framework/browse_thread/thread/27825e8c864add60/b7d99cf77cc4dd2c?lnk=gst&q=stateful#b7d99cf77cc4dd2c



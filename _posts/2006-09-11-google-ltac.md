---
layout: post
comments: true
permalink: /:title.html
title: 'Google LTAC'
author: Richard
date: 2006/09/11
alias: /google-ltac
tags:
---

<img src="/img/posts/flkexport2018/16176588905_f6636108ec_o.jpg" width="375" height="261" alt="4ff9dbc9abbf6-11219784-0-media_httpfarm3static_Jhnwk">

Google hosted a Test Automation Conference at their London offices on
the 7th and 8th of September. The videos of the presentations [are on Google Video][], so rather than do a blow-by-blow account of each talk,
I thought I'd note down what I saw as the main themes.

-   Domain languages — and literate programming. I saw a number of
threads where quite rich languages were used to express unit tests,
either built-in to systems (like Fit) or via a library for Java. The
literate side means writing tests along the lines of
`assertThat(thePage, has(selectBox().named("region")))`, with the
trick being of removing the dots and brackets to produce a test that
a customer or a business analyst could understand. I have my doubts
about the effort/reward of that, but I can see an advantage if you
can attach that style of writing to a story or requirement.
-   Performance measurement — looks like there's a lot to be gained from
running nightly benchmarks against code commits, so developers can
see when they've checked in something that hurts performance. That
would be a heck of a lot more effective than my approach of reaching
for the code profiler every now and then. But how do you know your
benchmarks reflect the performance of deployed systems running on
production hardware? You don't, but if you're the sort of
organization that uses many cheap machines in preferences to a few
large servers, you're in a better position.
-   Configuration and deployment — there's some interesting work going
on dealing with exploring large spaces of configuration options, and
in testing against deployed systems. To take the latter first, some
organizations strongly separate production machines from development
machines, which makes testing web services (as an example) difficult
because of firewall issues. That needs to change. Regarding
configuration, there's some smart work going on to automatically
explore the space of configuration options using some random and
some AI techniques. The study presented suggests that sampling the
space of options is a pretty good approximation to exhaustive
search, at least more so than you'd maybe guess.
-   Mobile — the conference kicked off with Shannon Maher pointing out
the difficulties of testing mobile applications (11,000 user agents
x connectivity options x carriers ... = combinatorial pain). The
take-away point from that presentation was that this is an important
problem to solve, with prizes for anyone who can solve it. Given
that this was Google's London director of mobile and director of
engineering talking, I think it's safe to assume that the prizes
aren't just toasters. But that was pretty much all that was said
about mobile from an application developer perspective, which was
surprising. Expect much more to be said in the future.

A few comments or thoughts that caught my attention: "any standard [W3C, OASIS, etc] without a test set doesn't exist"; a round of applause for
something described as being not XML; don't write huge chunks of
Javascript, instead separate your concerns as you would with other
languages; "the interesting thing about objects is the messages between
them, not their state".

If you only have chance to watch a couple of videos, catch:

-   [Goranka Bjedov on Using Open Source Tools for Performance Testing][]. Aside from it being great content on performance
testing, it gave me a sense of the kind of superstar bright people I
hear Google employ.
-   [Jason Huggins on Selenium][]. I could have done with a more basic
introduction to Selenium, but it's interesting to see how the
technology can be used to test a web UIs on multi-platforms in one
go as part of CruiseControl. Ok, the demo didn't work, but I have no
doubts it would.

The [slides and speaker bios][] are online too.

So, thanks to Google for arranging this, and to the host Allen
Hutchison: it feels like the start of a community. The size was good (at
150 people) and there was enough time between presentations to actually
talk to people and get to the bathroom and get some more coffee. Shame
it clashed with [d.Construct][], but in all other respects it was a
great success.

<img src="/img/posts/flkexport2018/16175835792_d04bf1cb87_o.jpg" width="421" height="321" alt="4ff9dbcb682e4-11219784-1-media_httpfarm3static_weiex">

Links:

-   Literate functional testing: [Literate Testing framework][], [JMock 2][].
-   Performance: [JMeter][], [Grinder][].
-   UI: [Rhino][], [Selenium][].
-   Configuration and deployment: [SmartFrog.org][], [GridUnit][],
[Jester][].
-   Photos: Flickr ["Google LTAC" search][], Flickr [group][].


  [are on Google Video]: http://video.google.co.uk/videosearch?q=London+Test+Automation+Conference
  [Goranka Bjedov on Using Open Source Tools for Performance Testing]: http://video.google.com/videoplay?docid=-6891978643577501895
  [Jason Huggins on Selenium]: http://video.google.com/videoplay?docid=-594153467742593805
  [slides and speaker bios]: http://www.google.co.uk/intl/en/events/londontesters/speakers.html
  [d.Construct]: http://2006.dconstruct.org/
  [Literate Testing framework]: http://code.google.com/p/literate/
  [JMock 2]: http://www.jmock.org/
  [JMeter]: http://jakarta.apache.org/jmeter/
  [Grinder]: http://grinder.sourceforge.net/
  [Rhino]: http://www.mozilla.org/rhino/
  [Selenium]: http://www.openqa.org/selenium/
  [SmartFrog.org]: http://www.smartfrog.org/
  [GridUnit]: http://gridunit.sf.net
  [Jester]: http://jester.sourceforge.net/
  ["Google LTAC" search]: http://www.flickr.com/search/?w=all&q=google+ltac&m=text
  [group]: http://www.flickr.com/groups/googleltc/


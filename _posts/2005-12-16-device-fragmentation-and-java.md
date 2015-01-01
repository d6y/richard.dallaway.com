---
layout: post
comments: true
permalink: /:title
title: 'Device Fragmentation and Java'
author: Richard
date: 2005/12/16
alias: /device-fragmentation-and-java
tags:
---

The topic at December's [London Mobile Monday][] meeting was "Device
Fragmentation and Java". I think it's another phrase for "diversity",
with a few "challenges":

1.  Different user interfaces: different screen sizes, for example.
2.  Different capabilities: some devices have Bluetooth support some
    don't; some devices will play some media formats, but not others;
    differences in performance characteristics...
3.  JCP specification that aren't tight enough, leading to different
    interpretations of the APIs.
4.  Configuration issues: handsets not configured for data access, or
    the operators messing with data as it goes through the network, or
    different behaviours depending on the tariff the customer is on
    (apparently).
5.  Bugs.

The first couple of issues aren't going to go away, and applications are
always going to have to adapt to the capabilities and the format of the
device. Is that such a bad thing? A big screen and a keyboard lends
itself to a different UI to a smaller touch screen. From the business
perspective, though, the question is: at what point is all the testing
going to kill you?

To some extend the second item, and definitely the third item, is being
addressed by the [MSA][] ([JSR-248][] and [JSR-249][]). It's all about
tightening up the specs—putting in more MUSTs and less of the SHOULDs
and MAYs—and making the TCKs more rigorous. That has the potential to
make a substantial difference, but don't hold your breath.

The last couple of issues will benefit from the development community
pushing back on the handset manufacturers and the operators. It's not
acceptable for, say, SonyEricsson to charge me for the privilege of
reporting bugs: sometimes you have to do it to bottom out an issue, but
it's not something I'm going to do unless I really have to. I'd like to
see less emphasis on the self-service forums, and an opening up the bug
databases. With that, though, comes the burden on developers to submit
simple reproducible bug reports.


  [London Mobile Monday]: http://www.mobilemonday.org.uk/
  [MSA]: http://www.forum.nokia.com/main/0,,7_20_49,00.html
  [JSR-248]: http://www.jcp.org/en/jsr/detail?id=248
  [JSR-249]: http://www.jcp.org/en/jsr/detail?id=249

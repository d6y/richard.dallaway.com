---
layout: post
comments: true
permalink: /:title
title: 'Sony Ericsson V800'
author: Richard
date: 2005/04/13
alias: /sony-ericsson-v800
tags:
---

<a href="https://www.flickr.com/photos/d6y/2049223809" title="v800 by Richard Dallaway, on Flickr"><img src="https://farm3.staticflickr.com/2018/2049223809_f7394803c7_o.jpg" width="275" height="291" alt="v800"></a>

It's been a few months since I moved away from a [P800][] to a [V800][].
I wanted a handset that had good enough PIM functions, that would [sync
with my Mac][], had the usual range of bands for roaming, but compared
to the P800 I wanted something I could use while walking (it turns out I
can't use the P800's stylus and move at the same time).

[The Register][] gave it a positive review, and I think it's fair. The
battery life is a little suspect, it's a little larger than it should
be, but what can you expect of an early generation 3G handset?

My biggest gripe is with the keypad: too many keys, and the space in the
wrong place. I can only guess that putting the space over on the right
under the 9 key is either for left-handed people or for two thumb
texting. For texting with one hand, when you're right handed, it's a
form of torture for your thumb. (The middle zero key is where the space
should be).

I wouldn't have touched the handset if it didn't come with a generous
selection of Java APIs: MIDP 2, CLDC 1.1, Nokia UI, WMA ([JSR-120][]),
MMAPI ([JSR-135][], for audio and video playback, and camera snapshot),
JTWI R1 ([JSR-185][]), KDWP, but not the bluetooth API ([JSR-82][]). The
applications we've written run fine on it, although I'll be re-reading
the RMS spec because I've made some gross assumptions about record
ordering in [Punch][].

The handset also supports SVG-T and Java 3D (JSR-184). As [we've][]
recently been working on some 3D data visualization, I'm looking forward
to see what the mobile Java 3D can do.

I'm not yet convinced by the clam shell design. On all the previous
phones I've owned, some part of the screen is always visible. That's
great for spotting a missed call or message. To get round the lack of
feedback, the V800 has a 2.2 inch TFT display in the lid of the phone.
This works fine, but feels a bloated solution. There must be a simpler
way.


  [P800]: http://developer.sonyericsson.com/site/global/products/phones/p800/p800.jsp
  [V800]: http://developer.sonyericsson.com/site/global/products/phones/v800/p_v800.jsp
  [sync with my Mac]: http://www.macosxhints.com/article.php?story=20041118143359172&query=v800
  [The Register]: http://www.theregister.co.uk/2004/11/12/review_sony_ericsson_v800/
  [JSR-120]: http://www.jcp.org/aboutJava/communityprocess/final/jsr120/
  [JSR-135]: http://www.jcp.org/aboutJava/communityprocess/final/jsr135/
  [JSR-185]: http://www.jcp.org/aboutJava/communityprocess/final/jsr185/
  [JSR-82]: http://www.jcp.org/aboutJava/communityprocess/final/jsr082/
  [Punch]: http://punch.sf.net/
  [we've]: http://www.spiralarm.com/

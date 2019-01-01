---
layout: post
comments: true
permalink: /:title.html
title: 'QuickTime for Java'
author: Richard
date: 2008/03/16
alias: /quicktime-for-java
tags:
---

A few months ago I was experimenting with [QuickTime for Java][]. It's
the binding between Apple's QuickTime "stuff" and the Java language,
allowing a Java developer to invoke QuickTime on the Mac (and presumably
also on Windows). The reason this appeals is that we do Java, and have
accumulated a fair amount of [Mac hardware][].

To cut a long story short, my limited experienced shows that QuickTime,
when compared to the standard Java image libraries, produced smaller
images of higher quality, faster. It's unusual to get all three benefits
together (smaller, faster, better). Too good to be true, even, which
leads me to think I've screwed up someplace, but I've not spotted it
yet.

The particular test that interested me was was taking a camera phone
photo, resizing it and rotating it. Here's an example original:

<img src="/img/posts/flkexport2018/16174715951_8c4004c911_o.jpg" width="375" height="500" alt="Jack">

Here's the rotation using tried-and-trusted [AffineTransform][] plus
[ImageIO][]:

<img src="/img/posts/flkexport2018/16109791797_6db6396f67_o.jpg" width="733" height="550" alt="affine">

And here's the same transformation run through QuickTime for Java using
the `GraphicsImporter` and `Matrix` objects:

<img src="/img/posts/flkexport2018/16294797722_594b7ebc20_o.jpg" width="733" height="550" alt="matrix">

Now, it's subtle but the QT4J image looks to have sharper colours, and
seems to be a better representation of the original input. It's also 20k
compared to the 76k using the Java 2D libraries, and the code runs 1.7
times faster. The downside: you need Apple hardware.

_Update: sadly these JPG images have been downloaded and re-saved since this post was written, for various blog platform ports. The quality will have dropped._


A few conclusions: it seems the Java 2D code doesn't just fall through
to QuickTime on the Apple platform in any simple sense (which, I suppose
I might have naively expected). Second, I suspect there's a lot of
tuning that can be done in the Java 2D client usage to improve the
quality, but QT4J seems to have good defaults.

(Before you ask, the dog's name is Jack.)

  [QuickTime for Java]: http://developer.apple.com/quicktime/qtjava/
  [Mac hardware]: http://www.apple.com/xserve/
  [Media\_httpfarm2static\_dkjdl]: ./images/11219752-0-media_httpfarm2static_dkjdl.jpg.scaled500.jpg
  [AffineTransform]: http://java.sun.com/j2se/1.4.2/docs/api/java/awt/geom/AffineTransform.html
  [ImageIO]: http://java.sun.com/j2se/1.4.2/docs/api/javax/imageio/ImageIO.html
  [Media\_httpfarm4static\_ctdbz]: ./images/11219752-1-media_httpfarm4static_ctdBz.jpg.scaled500.jpg
  [Media\_httpfarm4static\_wfnbh]: ./images/11219752-2-media_httpfarm4static_wFnBH.jpg.scaled500.jpg

---
layout: post
comments: true
permalink: /:title
title: 'Common Development and Distribution License'
author: Richard
date: 2005/05/25
alias: /common-development-and-distribution-license
---

This blog is about software licensing based on my personal reading of
license agreements and should not be taken as advice of any kind. If you
intend to act or not act based on the contents of this blog you do so at
your own risk.

The [OSI have approved][] Sun's [Common Development and Distribution License (CDDL)][] so I thought I'd take a look and see what [all the fuss][] is about.

When I license software I've usually gone for the [GPL][], occasionally
the [LGPL][], or a custom commercial license (which brings joy to
[our][] solicitors).

CDDL is new for me. It's [based on][] the [Mozilla license][] (MPL).
Compared to the GPL, I find the Mozilla license to be a tough read --
due to the phrasing, the number of parties involved and the way it
extends into other areas of "[intellectual property][]". So the place to
start with this stuff is Andrew M. St. Laurent's wonderful book
[Understanding Open Source and Free Software Licensing][]. It deals with
the MPL but was published before the CDDL arrived.

Here's a summary, ignoring all the details, of how I think of the
licenses (so please take a moment to re-read my disclaimer at the start
of this blog). The GPL says: *where ever you use this source code, the
resultant project must also be made available in source form for anyone
to use under the terms of the GPL.* My gut reaction here is that this is
what I mostly want for things I give away for free: you can't "steal" my
software and put it into a commercial product.

In contrast, the MPL says: *if you modify the source code, document your
changes, give us credit, make the modifications available as source, but
you can use the source in another project and license that larger work
however you want.* This is quite a leap: here's my freely donated code,
if you make it better I want to see the changes, but otherwise go ahead
and commercialize it or do whatever you want with it. In some senses
that's making the code more valuable and giving more freedoms than GPL.
I'm leaning towards this style of license now.

The CDDL says: *this is the MPL but cleaned up so you can use it without
having to resolve disputes in, and only in, California.*

It's important to note that source licensed under the GPL cannot be
mixed with source licensed under MPL or the CDDL -- but see the FSFs
[comments][] on various licenses for more information. This means you
need to decide where you stand on the various freedoms offered by the
various licenses, or get into [dual or triple licensing][] and
everything that [entails][].


  [OSI have approved]: http://developers.slashdot.org/article.pl?sid=05/01/19/1526226
  [Common Development and Distribution License (CDDL)]: http://www.sun.com/cddl/
  [all the fuss]: http://blogs.sun.com/roller/page/webmink/20050415
  [GPL]: http://www.gnu.org/copyleft/gpl.html
  [LGPL]: http://www.gnu.org/copyleft/lesser.html
  [our]: http://www.spiralarm.com/
  [based on]: http://www.sun.com/cddl/CDDL_why_details.html
  [Mozilla license]: http://www.mozilla.org/MPL/MPL-1.1.html
  [intellectual property]: http://www.gnu.org/philosophy/not-ipr.xhtml
  [Understanding Open Source and Free Software Licensing]: http://www.oreilly.com/catalog/osfreesoft/book/
  [comments]: http://www.fsf.org/licensing/licenses/license-list.html
  [dual or triple licensing]: http://www.mozilla.org/MPL/relicensing-faq.html
  [entails]: http://www.mozilla.org/MPL/boilerplate-1.1/mpl-tri-license-c

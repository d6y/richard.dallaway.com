---
layout: post
comments: true
permalink: /:title.html
title: 'NetBeans, Mobility, Mac'
author: Richard
date: 2006/07/13
alias: /netbeans-mobility-mac
tags:
---

The NetBeans [mobility pack][] includes a [cute][] drag-and-drop Java ME
application builder. Since day one I've been asking: does it run on a
Mac yet? Does it run on a Mac yet? Does it run on a Mac yet?...and the
answer has been "no". Running it across the network via X11 or similar,
or running inside a Windows emulator, isn't ideal so I've more-or-less
avoided NetBeans.

However, blogs from [Lukas Hasik][] and [Florian Beer][] pointed out
that you can hack the mobility downloads to make it run on Mac OS X, so
I took the plunge with it under NetBeans 5.5 beta with the enterprise
pack. And sure, it works:

<a href="https://www.flickr.com/photos/d6y/2050010326" title="Netbeans on Mac by Richard Dallaway, on Flickr"><img src="https://farm3.staticflickr.com/2383/2050010326_d942fa9ea0_o.png" width="525" height="343" alt="Netbeans on Mac"></a>

A couple of differences from the blog instructions: I didn't add a JDK
class set to my path when setting up the platform, I added the MIDP API
zip (because you don't want to try to use JDK 1.5 code on a handset just
yet). Oddly, the Hello World Midlet that NetBeans generated was broken,
and I've seen a few `IndexOutOfBounds` exceptions using the IDE, but I'm
using beta software, so that's fair enough.

I'm not overly enamoured with the code generation, but perhaps I'll grow
to love that as I customize it. The nice thing is about the mobility
pack is the easy-to-use "press the run button to compile, preverify and
run the application". Behind the scenes it's running ant, which is what
I do anyway, but somehow just having the button to press to do it for
all you makes a difference.

Looking ahead to [Mobility Pack 6][] there's at least the possibility
that when the mobility pack is open sourced we'll get a ZIP
distribution, but probably not full support for Mac OS X. If this is an
issue you care about, take a moment to register at [Netbeans.org][] and
then [cast a vote for issue 53076][]. Take care with the voting, because
it's not just a matter of clicking the "vote for this issue" link: after
that, you need to scroll down, enter a number in the text field opposite
the 53076 entry, then scroll some more and click submit.

UPDATE 2006-11-16: For the Netbeans 5.5 final release I also had to edit
`NetBeans.app/Contents/Resources/NetBeans/etc/netbeans.clusters` and add
`mobility7.3` to the end of the list.


  [mobility pack]: http://mobility.netbeans.org/
  [cute]: http://www.netbeans.org/kb/50/quickstart-mobility.html
  [Lukas Hasik]: http://blogs.sun.com/roller/page/lukas?entry=mobility_pack_on_mac
  [Florian Beer]: http://blog.no-panic.at/2006/04/13/j2me-development-on-netbeans-50-in-mac-os-x/
  [Media\_httpfarm3static\_innch]: ./images/11219787-0-media_httpfarm3static_inncH.png.scaled500.png
  [Mobility Pack 6]: http://blogs.sun.com/roller/page/lukas?entry=what_you_look_forward_in
  [Netbeans.org]: http://www.netbeans.org/
  [cast a vote for issue 53076]: http://www.netbeans.org/issues/show_bug.cgi?id=53076

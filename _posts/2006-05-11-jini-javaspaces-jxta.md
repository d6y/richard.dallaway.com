---
layout: post
comments: true
permalink: /:title.html
title: 'Jini, JavaSpaces, JXTA'
author: Richard
date: 2006/05/11
alias: /jini-javaspaces-jxta
tags:
---

...the holy trinity of cool technologies. Yet also the perennial
underachievers.

I worked with Jono on a Jini/JavaSpaces application for a publisher a
while back, and I think it was pretty successful. Certainly it worked
well enough, and when the corporate compliance and review police dropped
in to do a standard check-up they liked what they saw. Since then,
though, it seems to be "risky" to look at these technologies, and
instead messaging queuing (and now web services) are a safer bet.

For interoperability, SOAP and REST are the sane choices, but that
doesn't preclude using Jini and JavaSpaces behind the scenes. In some
ways the real competition is the Java EE stack, and it'd be interesting
if Jini and JavaSpaces were ever included in that specification.

One of my favourite features of these technologies is the ability to
find services without regard to where they are, how you get them, or how
they work (as opposed to having to know where something is and ask for
it). Frameworks like Spring, and dependency injection in general, are
bringing this style of programming into Java (that's not technically
precise, but it's sort of right in spirit).

I mention all of this now because there's been a bunch of news items
featuring these technologies. First, the Jini starter kit has [moved to an Apache license][] and may become an apache.org project. This could
really help kick this technology forward—particularly in marketing,
positioning and developer acceptance. JavaSpaces is probably still best
known in the context of grid computing, and as you'd expect [Sun are using Jini and JavaSpaces][] in their Grid Computing offering.
Elsewhere, [Gigaspaces][] and [Paremus][] are championing commercial
JavaSpace deployments. As for JXTA, it's now five years old, has a
[mobile version][], but I don't think I "get" P2P enough to actually
make serious use of this one yet. I must try harder on that front.

If you're interested in trying any of this out, here are some places
I've found useful:

-   [Jini starter kit][]
-   [Jan Newmarch's Guide to Jini Technologies][]
-   [JXTA community][]


In books, these are pretty much the classics:

-   [JavaSpaces Principles, Patterns, and Practice][]
-   [The Jini Specification][]
-   [JXTA in a Nutshell][]

Update 2006-10-09: the [Java Posse][] podcast has posted [one][],
[two][], [three][] excellent MP3 interviews with Van Simmons on the
subject of Jini and JavaSpaces. They also linked to the important [Note on Distributing Computing][] and the [Eight Fallacies of Distributed Computing][].

  [moved to an Apache license]: http://www.sun.com/software/jini/licensing/
  [Sun are using Jini and JavaSpaces]: https://computeserver.developer.network.com/
  [Gigaspaces]: http://www.gigaspaces.com/
  [Paremus]: http://www.paremus.com/
  [mobile version]: http://jxme.jxta.org/
  [Jini starter kit]: http://starterkit.jini.org/
  [Jan Newmarch's Guide to Jini Technologies]: http://jan.netcomp.monash.edu.au/java/jini/tutorial/Jini.xml
  [JXTA community]: http://www.jxta.org/
  [JavaSpaces Principles, Patterns, and Practice]: http://www.amazon.co.uk/exec/obidos/ASIN/0201309556/richarddallaway
  [The Jini Specification]: http://www.amazon.co.uk/exec/obidos/ASIN/0201726173/richarddallaway
  [JXTA in a Nutshell]: http://www.amazon.co.uk/exec/obidos/ASIN/059600236X/richarddallaway
  [Java Posse]: http://javaposse.com/
  [one]: http://media.libsyn.com/media/dickwall/JavaPosse082.mp3
  [two]: http://media.libsyn.com/media/dickwall/JavaPosse084.mp3
  [three]: http://media.libsyn.com/media/dickwall/JavaPosse086.mp3
  [Note on Distributing Computing]: http://research.sun.com/techrep/1994/abstract-29.html
  [Eight Fallacies of Distributed Computing]: http://weblogs.java.net/jag/Fallacies.html


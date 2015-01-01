---
layout: post
comments: true
permalink: /:title
title: 'Persistence options'
author: Richard
date: 2005/04/07
alias: /persistence-options
tags:
---

When it comes to writing an application that uses a relational database
(and that's pretty much all of them, for me), I like to use a tool that
hides the details of the database I'm using, and reduces the tedium and
error involved in mapping code to database structures. For me, that was
[JDO][] - a Java standard that has a great open source implementation in
terms of [Jpox][].

The goal of picking a persistence framework is, amongst other things, to
end up using one that isn't going to be obsolete. This is, of course,
impossible. Sooner or later everything becomes obsolete. With JDO going
into a version 2 specification process, having gained plenty of vendor
and open source support, and also being well thought-out to start with,
it looked like a good horse to back. But no.

Some consider there to be too many ways to do persistence in Java, and
something had to give. Sun's [Letter to the Java Technology Community][]
makes it clear that JDO isn't the one to back. What we want, apparently,
is a single persistence framework for both J2EE and J2SE. That's a good
thing, but it's not ready yet.

The Thing That Replaces JDO is going to be specified as part of the EJB
3 process, so you can guess that EJB 3 is going to have a big influence
on The Thing That Replaces JDO. I should make it clear that this doesn't
technically kill JDO. The [interview with the JDO spec lead][], Craig
Russell, shows that there's a commitment to support JDO and for vendors
to help migrate us over to The Thing That Replaces JDO. But still, who's
going to invest their time in using JDO when it's already flagged as
obsolete?

While this plays out, then, it seems that the only good bet is going to
be [Hibernate][]. It's not based on a community specification, but it is
open source, has lots of users, and has been around for quite a few
years. It's the one I'll be using.

[Paul][]'s been using it for a while, and suggested I look at the
[Hibernate in Action][] book. There's also a long but useful
presentation over at [Javapolis][]. So far I'm impressed: it has that
battle-hardened feel to it that gives you confidence that someone else
has already suffered the pain in finding and fixing any nasty problems.


  [JDO]: http://www.jdocentral.com/
  [Jpox]: http://www.jpox.org/
  [Letter to the Java Technology Community]: http://java.sun.com/j2ee/letter/persistence.html
  [interview with the JDO spec lead]: http://www.jdocentral.com/JDO_Commentary_CraigRussell_3.html
  [Hibernate]: http://www.hibernate.org/
  [Paul]: http://www.goulbourn.com/
  [Hibernate in Action]: http://www.manning.com/bauer
  [Javapolis]: http://www.javalobby.org/av/javapolis/

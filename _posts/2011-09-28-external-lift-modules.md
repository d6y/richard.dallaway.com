---
layout: post
comments: true
permalink: /:title
title: 'External Lift Modules'
author: Richard
date: 2011/09/28
alias: /external-lift-modules
tags:
---

An external module is one way to share lumps of code with the Lift
community.  The word "external" indicates that these are distinct from
the modules in the core Lift repository maintained by Lift comitters.
 But the pattern to create a module is the same, as [Peter][] [outlined on the Lift Wiki][] and in [one][] or [two][] posts on the Lift mailing
list.

Anyone can create one, anywhere they like, and people do. So there's
nothing to talk about.

Except... I'd like to have modules build automtically when Lift changes,
with reports on problems, and have an easy way to find modules and add
them to Lift projects.  If you buy into that as something useful, there
are lots of ways to achieve it.  

**Are we there yet? Are we there yet? Are we...**

Today, we're nowhere near, but we've started by experimenting with a
build system and a common repository for external modules. 

We want to track Scala and Lift releases, including Lift milestone
releases, and publish these builds so they can be available quickly to
anyone wanting to use them via Maven or SBT.  That's the plan.  To
explore this we're using [CloudBees][]. Under [their open source programme][] we're given a hosted Jenkins instance and repository for
build artifacts.  In other words, we go to the CloudBees web site,
create a new Jenkins job, point it at the appropriate GitHub repo and
press "Build now". Nice and easy, but all manual.

Currently we're building just a few modules, which you can see on the
[Jenkins dashboard][]:

-    [Google Analytics][] support
-    [IMAP IDLE][], providing push-like email into a Lift app
-    [JqPlot module][] for graphs

...but none of it is automatic yet.  We know we can do better.

**The developer experience**

This is what the current development process is like for getting a new
release out.  Let's say, Lift 2.4-M4 becomes available ([which it
has][]; this post has been in draft too long).

The module versioning number I'm using is the Lift version with the
module version appended.  So the 0.91 version of the IMAP module for
Lift 2.4-M3 is: 2.4-M3-0.91. Sometimes, that's all you need to change,
making me wonder if this is a wasteful system.  Anway, let's publish a
version of 2.4-M4:

` $ cd liftmodules-imap-idle $ git checkout -b 2.4-M4 $ vi build.sbt // modify version, liftVersion, test $ git push origin 2.4-M4 $ sbt   > +publish   `

...then merge back to master:

` $ git checkout master $ git merge 2.4-M4 $ git push origin master   `

**Where next?**

Are we making a meal out of this?  Is this something worth doing?  Are
there better ways?  Feedback is what we need, and if you're part of the
Lift community, the place for that is on [the mailing list][] please.

Otherwise we'll be investigating: 

-   adding triggers or polling to trigger builds of modules. 
-   possibly a directory of modules - but that seems like [the easiest part][] now that we've started to push out notifications to the fantastic [implicit.ly][] space.
-   scratching our heads about how to track SNAPSHOP.
-   can we automate module changes to track Lift milestone releases?
-   automatically publishing builds into the repo.

Or something like that.

If you want your module included in this build process, hassle me or
Jono on the Lift mailing list.


![Built on CloudBees](//www.cloudbees.com/sites/default/files/Button-Built-on-CB-1.png)

 

 

  [Peter]: http://twitter.com/#!/pr1001
  [outlined on the Lift Wiki]: http://www.assembla.com/spaces/liftweb/wiki/Modules
  [one]: http://groups.google.com/group/liftweb/browse_thread/thread/9f330b56a778d2f2?fwc=1
  [two]: https://groups.google.com/d/msg/liftweb/GfVqg7hHmYA/wux_RABpj74J
  [CloudBees]: http://www.cloudbees.com
  [their open source programme]: http://cloudbees.com/foss/index.cb
  [Jenkins dashboard]: http://liftmodules.ci.cloudbees.com
  [Google Analytics]: https://github.com/d6y/liftmodules-googleanalytics
  [IMAP IDLE]: https://github.com/d6y/liftmodules-imap-idle
  [JqPlot module]: https://github.com/jonoabroad/liftmodules-jqplot
  [which it has]: http://lift.la/announcing-lift-24-m4
  [the mailing list]: http://groups.google.com/group/liftweb
  [the easiest part]: http://implicit.ly/tag/netliftmodules
  [implicit.ly]: http://implicit.ly/
  [Builtondev]: ./images/72915424-0-BuiltOnDEV.png.scaled500.png

---
layout: post
comments: true
permalink: /:title
title: 'Updated Google Analytics Lift module for EU cookie law'
author: Richard
date: 2012/06/10
alias: /updated-google-analytics-lift-module-for-eu-c
tags:
---


Since [I last wrote about this](http://richard.dallaway.com/jsessionid-and-the-like-plus-the-privacy-and), the UK [guidance for compliance (PDF](http://www.ico.gov.uk/for_organisations/privacy_and_electronic_communications/the_guide/~/media/documents/library/Privacy_and_electronic/Practical_application/cookies_guidance_v3.ashx)) has been updated to note that "implied consent is a valid form of consent" for setting cookies.  You may have noticed, as a consequence, web sites changing how they inform users about cookie policy.

Dave Evans from the Information Commissioner's Office, put it nicely when he says to review user cookie information to "make it more prominent, make it more user-friendly, make it mean something to [end users]" and that it's about "being clear and honest up-front about cookies":

<iframe width="480" height="270" src="https://www.youtube.com/embed/V0M8MYiGkQw?rel=0" frameborder="0" allowfullscreen="allowfullscreen"> </iframe>

That's part of compliance, and it's the responsibility of sites to ensure they understand the requirements and comply. All the details are over at
[http://www.ico.gov.uk/cookies](http://www.ico.gov.uk/cookies).

What I've done is update the Google Analytics module for the [Lift web framework](http://www.liftweb.net) to help with the control and communication part.  It's not that the law is just about analytics, or even just restricted to cookies, but for us Google Analytics are the most problematic cookies that we use, so it seems sensible to place the code on that module.

**What's changed**

There are two changes.  The first is the obvious one: you can now provide a function to control if the Google Analytics module will include tracking code on your site or not:

<script src="https://gist.github.com/2906122.js"> </script>

The `only when` part is because some of you may need to be showing code to compliance teams and would like something legible for civilians. You can just supply a `()=>Boolean` to `init` if you prefer.

You have access to `S` so you can pretty much test for anything you like (cookies, requests, database values).  The default is to always include analytics.

That small change should allow you to turn analytics on or off on a per-user or per-request basis, so you can go implement nice controls and notices, maybe using snippet and specific locations on your pages to show information.

However, the second change lets you fling some JavasScript around to notify users of your cookie policy without having to change your site. You may prefer not to use this and implement your notices some other way.

<script src="https://gist.github.com/2906148.js"> </script>


That's a horrible example.  Let's try something better...

**Example implementation**

The BBC have done a lot of work, as explained on their [Privacy and Cookies Internet blogpost](http://www.bbc.co.uk/blogs/bbcinternet/2012/05/privacy_cookies_ico.html), one part of which is to put a big banner on their site when you visit it:

<a href="http:/d6y.trovebox.com/p/s"><img width="600" alt="Screen grab of BBC web site showing cookie info banner" src="http://d6y.trovebox.com/photo/s/create/f240c/870x550.jpg"></img></a>

They show this banner and set a couple of ckns_policy cookies.  Once you click to another page on the site, the banner is gone.  The Guardian [does something that produces a similar effect](http://d6y.trovebox.com/p/t), only involving a scary number of cookies.

Here's one way to achieve that approach with the module:

<script src="https://gist.github.com/2906159.js"> </script>

Effectively, it's giving you the ability to trigger a `()=>JsCmd` function on some condition. In this case, we're adding a _div_ a the top of the page using a bit of JQuery.  That's maybe not for you, but it provides a single point to add this policy concern across your whole site.

Full details about the module can be found on the [module's Github page](https://github.com/d6y/liftmodules-googleanalytics). It's available now as a 1.0-SNAPSHOT version for Lift 2.5-SNAPSHOT. Your feedback is welcome.


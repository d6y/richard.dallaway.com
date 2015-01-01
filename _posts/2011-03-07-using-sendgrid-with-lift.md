---
layout: post
comments: true
permalink: /:title
title: 'Using Sendgrid with Lift'
author: Richard
date: 2011/03/07
alias: /using-sendgrid-with-lift
tags:
---

If you want to send email from your [Lift][] app using [SendGrid][] you
need to do two things: set an authenticator and enable the javax.mail
authentication flag. That second part isn't obvious.

**Step by step...**

Assuming your Lift props file contains the following:

<script src="https://gist.github.com/3072157.js"> </script>

In Boot you need to set the authenticator:

<script src="https://gist.github.com/3072181.js"> </script>
 
And you're done.  The regular `Mailer` class will now use SendGrid.
  [Lift]: http://liftweb.net/
  [SendGrid]: http://sendgrid.tellapal.com/a/clk/7yTzZ

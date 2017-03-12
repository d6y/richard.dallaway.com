---
layout: post
comments: true
permalink: /:title.html
title: 'Option(null) is None'
author: Richard
date: 2011/04/03
alias: /optionnull-is-none
tags:
---

[@wfaler asked][]:

*Doing a presentation soonish on "Why \#Scala" - have a truckload of
arguments, but what are your arguments for Scala over Java? Give me
ammo!*

Ok, I'll bite.  Here's one of those every-day things I stop and notice
from time-to-time. It's not new, it's not huge, it's just one of the
things that makes a difference when you're trying to ship stuff.

The Google Java APIs for accessing Google contacts has you write code
like this:

<script src="https://gist.github.com/3072110.js"> </script>

That's how the ["retrieving all contacts" example][] starts.  The basic
pattern is: check if the property you want is available, then get it. 

I recently had the need to go through contacts to extract birthdays.  I
only care about contacts with a name and a birthday, but I use Scala so
don't have to do the if/get dance because [Option does the right thing in a for-comprehension][]:

<script src="https://gist.github.com/3072136.js"> </script> 

The above evaluates to a `List[String]` (because `contacts` starts as a
`List`) containing those contacts with a name and a birthday.  So even
when using regular Java libraries, you end with benefits (less code,
more readable code, less-likely-to-cock-up-the-logic code) just by using
Scala. IMHO.

  [@wfaler asked]: http://twitter.com/#!/wfaler/status/53133339531550720
  ["retrieving all contacts" example]: http://code.google.com/apis/contacts/docs/3.0/developers_guide_java.html#retrieving_without_query
  [Option does the right thing in a for-comprehension]: http://programming-scala.labs.oreilly.com/ch13.html#OptionsAndForComprehensions



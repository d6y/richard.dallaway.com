---
layout: post
comments: true
permalink: /:title.html
title: 'Functional Brighton: &#34;What functional programming means to me&#34;'
author: Richard
date: 2011/05/29
alias: /functional-brighton-presentations-on-what-fun
tags:
---

The [Functional Brighton meetup][] for May was a set of short demos and
talks on the subject of "What functional programming means to us".
 [Kit][] kicked off the evening with an F\# show-and-tell of a 3D
flocking simulator (complete wth 3D glasses); [Eric][] spoke about why
he uses Haskell; Andy showed us some Scheme code and made it
progressively more functional; and [Tom][] jumped up with a code tour of
a Haskell project of his.  A good evening with plenty of Q&A.

I had every intention of recording what I said about Scala, but failed.
 So here goes with a reconstruction of what I said or intended to say.

<script async class="speakerdeck-embed" data-id="4e81c4da22cb44006301263f" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

"What functional programming means to me" has been a fun assignment. I
have no background in functional programming—and the funky languages I
have used for any period of time were never described as functional.  
So the way I've approached this is to report on three things I've
noticed during my transition from mostly-Java to Scala, and consequently
I'm not sure how much of this is really about functional programming.  

**Approach to problem solving**

The first of the three things is very general. It's about how I seem to
approach problems.  It's also the hardest one for me to explain, and
probably the most waffly.

Here (slide 2) is an example of the kind of problem I had to deal with
last year, and what we're doing is taking a tweet and adding a hashtag
to it if it's not already there.  I'm not sure how I used to solve these
problems, but I think it would have dived in with a loop and maybe a
string buffer and I'd test and append.

Now what I seem to find myself doing is thinking about what I have, and
what I want to get (slide 3).  What do I have? Some tags and a tweet.
 What do I want to get back? A tweet. Which really means (slide 4) I
have a list of strings and a string, and I want a string back.  By
thinking about what types are involved, I find myself asking what kinds
of transformations will get me the result I want.

So in code (slide 5) if we have a List[String] and a String, and we want
to get a String back, then a fold is a reasonable choice.   This is
Scala, we're defining appendTags which takes our tweet which is of type
String, and the tags of type List of String, and it returns a String
(although I've left that off as it's being inferred).  

AppendOne solves the simple case: it expects a string and a tag and does
the obvious thing.  As luck would have it, the two parameters appendOne
wants are the parameters that foldLeft gives it for each of the tags and
the current tweet. 

(At this point I think we dropped to the REPL and tried this out)

I find these solutions preferable in terms of clarity, LOC, also
happiness in not writting the same old loop I've written many times
before (slide 6).

That was the first thing: thinking in terms of transformations, and how
it sort of suggests solutions in terms of functional programming
constructs.

**Concurrency**

The second thing is about concurrency.  From what I recall, in C it was
just plain hard to write concurrent code at all.  In Java, it's easy,
but even easier to get it wrong.   It looks to me that the functional
world want to address this problem, being both correct and easy to use.
 Maybe this quote (slide 7) alludes to why, as concurrent problems boil
down to coordinated access to shared mutable state.  As functional tends
to point you away from mutable state, part of the battle is over.  

These are facilities I take advantage of today, and it makes it easy to
bring not-insubstantial improvements to applications.   I have a code
base (slide 8) that makes multiple HTTP requests to web APIs.  In this
example the function yahoo, if you call it, hits Yahoo's site and brings
back address book entries (let's say).  And the google one does the same
to Google's API site.   They don't take very long, but they absolutely
can happen concurrently.

I have a list of two functions, the underscore telling us the functions
aren't actually being called yet.  I turn them into a parallel
collection and then call the apply function on them.  They are executing
in parallel and the result will be a List containing two lists: one for
the results from each site.

This is one-liner stuff.  There's no barrier to going concurrent.

Actually, that example is from Scala 2.9, and I don't use Scala 2.9 yet,
so I do something a little more involved (slide 9).

**Useful concepts**

The third thing that "functional is" to me, is the set of constructs
functional programming assumes.  Facilities like Option, comprehensions,
flatMap, pattern matching which, as I've said here (slide 10), are
astonishingly useful every day and yet I'd never heard of them before
using Scala.

Initially they seem like esoteric, academic, hard to understand.
 They're not hard: they're rich. It takes time to appreciate them.  But
it's time well spent.

I've [blogged about this example][] (slides 11 and 12), but it's
accessing the Google contacts API.  Comparing the Google Java code and
the the corresponding use of Option and for comprehensions are much
easier to read, and easier to write.  They make a big impact in
day-to-day code.

**Summary**

Those were the three things (slide 13) that functional programming
appears to mean to me at the moment.

1.  Thinking in terms of transforamtions changed my approach to problem
    solving for the better.
2.  Concurrency without any barriers to use.
3.  The facilities in functional programming that let you write better
    programmes.

 

 

  [Functional Brighton meetup]: http://www.meetup.com/Functional-Brighton/
  [Kit]: http://www.linkedin.com/pub/kit-eason/4/688/627
  [Eric]: http://erickow.com/
  [Tom]: http://almostobsolete.net/
  [blogged about this example]: http://richard.dallaway.com/optionnull-is-none%20

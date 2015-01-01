---
layout: post
comments: true
permalink: /:title
title: 'Getting Started with Scala'
author: Richard
date: 2008/02/03
alias: /getting-started-with-scala
tags:
---

Friday night was the first London [Scala][] User Group meet-up, so it
seems like the right time to say what I've learned so far about [the language][].

I'm not a functional programming person. My background, pre-Java and C,
was dominated by [POP-11][] so it's fair to say I'm more comfortable
with [imperative coding][] than anything else. But that's kind of why
Scala's a serious consideration for me: it doesn't force the functional
stuff down your throat, but rather it's all there, object and
functional, so you can pick and choose.

Other compelling aspects of the language are: it runs on the JVM, which
has had a huge engineering investment to make it as fast as it is today;
has Java-like features, but looks like it lets you get your work done
with less lines of code; and it's strongly typed. In other words, it's
comfy for a Java person, while putting you in a position to use the
funky actors and functional stuff, and maybe make even better use of
those multi-core machines.

Here's an example of less-lines-of-code thing. In Java, the type-safe
idiom for iterating over a map is:

	final Map<String, String> map  = new HashMap<String, String>();
	map.put("dog", "woof");
	map.put("cat", "meow");

	for(Map.Entry<String, String> entry : map.entrySet())
	{
	  final String name = entry.getKey();
	  final String value = entry.getValue();
	  // do something with name and value here
	}

The same code, just as strongly typed, in Scala is:

	val map = Map[String,String]("dog" -> "woof", "cat" -> "meow")
	for( (name,value) <- map) {
	 // do something with name and value here
	}

Or, if you wanted that last part could be:
`map.foreach( (pair) => ...do something with pair._1 and pair._2 here.. )`.

If you're interested in getting into this stuff, I found it useful to
start with [A Scala Tutorial for Java programmers PDF][], then listen to
[Episode 62 of Software Engineering Radio][], an interview with Martin
Odersky on Scala (thanks to [Dom][] for pointing that out to me).

After that, I think the best place to go is into your wallet and buy a
copy of [Programming in Scala: A comprehensive step-by-step guide][].
It's a pre-print, with quite a few errors, but I'm pretty sure they'll
get cleaned up soon and it'll be a great text.


  [Scala]: http://en.wikipedia.org/wiki/Scala_%28programming_language%29
  [the language]: http://www.scala-lang.org/
  [POP-11]: http://en.wikipedia.org/wiki/POP-11
  [imperative coding]: http://en.wikipedia.org/wiki/Imperative_programming
  [A Scala Tutorial for Java programmers PDF]: http://www.scala-lang.org/docu/files/ScalaTutorial.pdf
  [Episode 62 of Software Engineering Radio]: http://www.se-radio.net/podcast/2007-07/episode-62-martin-odersky-scala
  [Dom]: http://happygiraffe.net/blog/
  [Programming in Scala: A comprehensive step-by-step guide]: http://www.artima.com/shop/forsale


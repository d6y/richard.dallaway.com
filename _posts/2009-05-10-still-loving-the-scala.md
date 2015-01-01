---
layout: post
comments: true
permalink: /:title
title: 'Still loving the Scala'
author: Richard
date: 2009/05/10
alias: /still-loving-the-scala
tags:
---

<a href="http://www.flickr.com/photos/janed/3484475728/"><img src="http://farm4.static.flickr.com/3392/3484475728_abbde18362.jpg"/></a>

The other night I sat down to satisfy just one more quick-fix
screen-scraping twitter-based itch. Of course I decided to scratch using
Scala, and it really is an attractive language for "doing stuff". It
also made me reflect on how Scala has already started to changed the way
I write software.

Here's the itch: If you live in the centre of the pebble-beach city of
Brighton, and have a dog, the tide times become really interesting,
because at low tide sand is revealed, and [sand is fantastic to play
fetch in][]. You can get [Brighton tide times from VisitBrighton.com][],
but of course I want the information at the point I'm going to use it.
And for me, that means I want a tweet at 6:30 every morning. And so,
[@brightontide][] was born (I'm still chatting about copyright with the
council, so it might have to disappear).

The problem could be boiled down to curl -\> grep/sed/awk -\> curl, but
there's the added complication that tide times are in GMT, and I want to
tweet them corrected for daylight saving.

So with that background out of the way, here we go...

In essence I want to get the tide times for a given date:

<script src="https://gist.github.com/3156851.js"> </script>

I'm using the [Joda Time][] classes `LocaDate` and `LocalTime` to
represent a date (without time) and a time (without a date). The `Tide`
class is just a wrapper for the time and height of the tide, plus the
method for converting the time into the right timezone:

<script src="https://gist.github.com/3158986.js"> </script>

Next I need to implement a `TideSource` and, until I find something less
hacky, that will be an implementation that scrapes the data from
VisitBrighton.com. I want to use it like this:

    val tide_times = VisitBrightonScraper.lowsFor(today)


Here's the code to allow that:

<script src="https://gist.github.com/3159025.js"> </script>

There's nothing particularly exciting in any of this, but there are a
couple of things that surprised me. The first is the `zip` function,
which I distinctly remember reading about and thinking at the time:
"nice, but there's no practical application I'll write that will ever
need that" :-) But here we are, with a page layout where I have two
columns of numbers that need to be paired up: exactly what zip does, and
it saved me a couple of loops.

The regular expression is a bit hairy. You can figure out what's going
on if you view the source of the page:

<a href="http://d6y.trovebox.com/p/11"><img src="http://awesomeness.openphoto.me/custom/201207/68d025-11219733-0-media_httpfarm4static_pGgaD_870x550.jpg" width="480"/></a>


But that code is going to need a unit test. But how to "mock out" the
HTTP request? In Java, I probably would have separated things more so I
supply the content of the HTTP request to another method that processes
it. Or maybe I'd have a factory to supply something that can fetch the
HTML; or perhaps I'd use dependency injection.... but this is a quick
script, to do a hacky job... but I do want to test it. It turned out to
be so very easy:

    object MockScraper extends VisitBrightonScraper {
      override def page = Source.fromFile(
       "src/test/resources/visitbrighton07052009.html",
        "UTF-8").mkString
    }

Nothing you can't do in Java, but here it feels so concise and easy that
it something that becomes usable for a unit test.

Here's the corresponding test code that uses this test scraper:

    object VisitBrightonScraperSpec extends Specification {

      "Visit Brighton screen scraper" should {

        "locate low tide in first day" in {
          val tides = MockScraper.lowsFor(new LocalDate(2009, 5, 7)) tides.length must be_==(2)
          val expected = List( Tide(new LocalTime(3,38), Metre(1.0)), Tide(new LocalTime(15,59), Metre(1.0)) )

          tides must be_==(expected)
         }
         // etc
     }

Putting it together, you get:

    object TideTweet {

      def main(args:Array[String]) {

        val today = new LocalDate

        // Time tides are in GMT, but we will later convert to
        // whatever timezone we're in:
        val tz = DateTimeZone.getDefault
        val gmt_tides = VisitBrightonScraper.lowsFor(today)

        val tweet = gmt_tides match {
          case Nil   => "Gah! Failed to find tide times today.... Help!"
          case tides => today.toString("'Low tides for 'EE d MMM': '") +
                           tides.map(_.forZone(tz)).mkString(", ")
        }

        println(tweet)

        if (args contains "-dotweet") send(tweet)
      }

    }


I've skipped some of the details:[the source is on github][] [update:
it's evolved a little since this blog post].

This isn't exactly great code (error states returning `Nil`? Twitter
passwords baked into the source?) but it was an absolute joy and
pleasure to write it in Scala.


  [sand is fantastic to play fetch in]: http://www.flickr.com/photos/janed/sets/72157617444708930/
  [Brighton tide times from VisitBrighton.com]: http://www.visitbrighton.com/site/tourist-information/tide-timetables
  [@brightontide]: http://twitter.com/brightontide
  [Joda Time]: http://joda-time.sourceforge.net/
  [Media\_httpfarm4static\_pggad]: ./images/11219733-0-media_httpfarm4static_pGgaD.jpg.scaled500.jpg
  [the source is on github]: http://github.com/d6y/brightontide


---
layout: post
comments: true
permalink: /:title.html
title: 'Microformats'
author: Richard
date: 2007/04/05
alias: /microformats-14
---

Last night's [Sussex Geek dinner][] featured [Glenn Jones][] with a
presentation on Microformats. I learned a lot in a short space of time.

My previous impression of microformats was that it was a kludgy way to
piggyback contact and event details into HTML—clever and useful,
admittedly, but a kludge nonetheless. But I was totally wrong. I
recommend grabbing [the PDF presentation][] and taking a look for
yourself, but it turns out the attributes used to mark-up XHTML with
contact details and the like are, for want of a better word, legitimate.
For example, using the `class` attribute to identify a name of a person
is fine: the [XHTML spec][] describes `class` as good for pretty much
any use: "The class attribute can be used for different purposes in

XHTML, for instance as a style sheet selector (when an author wishes to
assign style information to a set of elements), and for general purpose
processing by user agents." Ah, so it's not just for CSS. Ok.

Adding to that are a couple more appealing features: the specs are
short, look pretty straightforward to start using, and are based on
existing standards such as vcard. The details are over at
[microformats.org][]. I'll add "[hReview][]-ifying [book write-ups][]"
to my to do list...one day....

For me the most interesting part of the talk was idea of designing URLs
so they look like API calls. The example given was along the lines of:
`http://somesite/tag/creative` can be thought of as
`somesite.tags.getList("creative")`. That works for me, and is probably
a good rule of thumb for designing URLs. The tricky part comes with
queries: how to incorporate do ANDs, ORs, ranges, etc. One option might
be to use a [builder pattern][], such as `/tags/creative/startingWith/c`
interpreted as `tags.getList("creative").startingWith("c")`. Another
option would be convention where by the order of the elements of the URL
have a meaning and you just need to know it. The example there would be
[http://traintimes.org.uk/brighton/london/08:30][] means "list trains
from Brighton to London, leaving Brighton at 08:30". Stuff to ponder.

Indeed, the evening left me with plenty of stuff to ponder: How does
this relate to RSS? How does this tie up with RESTful web services.
Stuff to ponder is a good sign, BTW.

Now we all need to go play with the [Operator plugin for Firefox][].

UPDATE: 19 April 2007, via [Jeremy's twitterings][], [Picoformats][]
look interesting.

Disclaimer: [Glenn Jones][] was founder and Creative Director at [Madgex][],
which was [Jane][]'s new employer at the time of writing.

  [Sussex Geek dinner]: http://sussex.geekdinner.co.uk/
  [Glenn Jones]: http://www.glennjones.net/about/
  [the PDF presentation]: http://www.glennjones.net/downloads/MicroformatsHTMLtoAPI.pdf
  [XHTML spec]: http://www.w3.org/TR/xhtml2/mod-core.html
  [microformats.org]: http://microformats.org/
  [hReview]: http://microformats.org/wiki/hreview
  [book write-ups]: http://www.dallaway.com/reading/current.xml
  [builder pattern]: http://en.wikipedia.org/wiki/Builder_pattern
  [http://traintimes.org.uk/brighton/london/08:30]: http://traintimes.org.uk/brighton/london/08:30
  [Operator plugin for Firefox]: https://addons.mozilla.org/en-US/firefox/addon/4106
  [Jeremy's twitterings]: http://twitter.com/adactio
  [Picoformats]: http://microformats.org/wiki/picoformats
  [Madgex]: http://www.madgex.com/
  [Jane]: http://jane.dallaway.com/about.html


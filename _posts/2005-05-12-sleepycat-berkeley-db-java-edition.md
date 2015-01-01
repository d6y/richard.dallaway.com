---
layout: post
comments: true
permalink: /:title
title: 'Sleepycat: Berkeley DB Java Edition'
author: Richard
date: 2005/05/12
alias: /sleepycat-berkeley-db-java-edition
tags:
---

I recently had a look at Sleepycat Software's [Berkeley DB Java
Edition][]. What's on offer is a transactional data store that can be
embedded inside an application, including inside web applications.
Compared to other embedded products, this one has been around forever,
so it should be bullet proof.

I think of the Berkeley DB as more-or-less a persistent hash table. One
object is a key, another is the content, and the rest is all `put` and
`get` against a transaction ("`extends TupleBinding`" is the magic
needed on a class). It's fast, it's reliable and it's simple to use. The
lack of external dependencies (no server to worry about) means it's a
doddle to write unit tests for. There is a little bit of
serialization/deserialization code to write, but even that's pretty
easy.

In fact, there are just two fatal downsides.

When I went on my first and only proper database training course (for
[Illustra][], which shares a history with [Postgres][]) I'm sure one of
the things I was told was: *SQL is about declarative data access*. You
get to say what data you want without worrying about how you get it.
That doesn't really sink in until you have to worry about the
how-you-get-at-it part. With Berkeley DB, you do have to worry about
that. For example, you're storing some data with one kind of key, and
now you want to access the data some other way. Tough. You need to
iterate over the data and figure it out, or implement an index for
secondary keys. Pain and hassle. If you reach that point it's probably
time to move to SQL.

The second problem is all about money. At first glance the [licensing terms][] look great, but digging into the definition of
"[redistribution][]" it becomes apparent that any commercial application
needs to buy a license. Fair enough, so how much? "For pricing
information, or if you have further questions on licensing, please
contact us." So I did, and we'd be looking at US$40k - US$150k before
annual support. Considering that price is not based on number of servers
or CPUs or any time period, it's not so bad. But not so good if you can
work with MySQL or PostgreSQL. (Other pricing is available, if you want
to talk to the Sleepycat sales team).

Summary: I found it rock solid and blindingly fast. It's a good product,
but we all have different trade-offs for what we do. I'd consider it for
specialized projects, but for the rest of the time I'll stick with SQL.

If you want to try it out, the [getting started guide (PDF)][] is a
well-written document worth reading.


  [Berkeley DB Java Edition]: http://www.sleepycat.com/products/je.shtml
  [Illustra]: http://c2.com/cgi/wiki?IllustraDatabase
  [Postgres]: http://archives.postgresql.org/pgsql-advocacy/2004-12/msg00033.php
  [licensing terms]: http://www.sleepycat.com/download/jeoslicense.html
  [redistribution]: http://www.sleepycat.com/download/licensinginfo.shtml
  [getting started guide (PDF)]: http://www.sleepycat.com/jedocs/GettingStartedGuide/BerkeleyDB-JE-GSG.pdf

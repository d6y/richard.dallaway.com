---
layout: post
comments: true
permalink: /:title
title: 'A Review of &#34;CouchDB: The Definitive Guide&#34;'
author: Richard
date: 2010/12/31
alias: /a-review-of-couchdb-the-definitive-guide
tags:
---

![cover][]

*[CouchDB: The Definitive Guide][]*, first edition, by J. Chris
Anderson, Jan Lehnardt and Noah Slater.

Published by O'Reilly Media, and [available online under a Creative Commons license.][]

## Summary

This first edition is inevitably out-of-date compared to 1.0 of CouchDB,
has room for improvement, but is nevertheless a useful introduction to
CouchDB and thinking about things in the CouchDB-way.  A pleasure to
read.

## Review

The sense I get of NoSQL products, such as CouchDB, is that they are
good for certain kinds of problem and have potentially great performance
characteristics.  I'm nowhere near pushing the capabilities of
relational databases, but I was interested in understanding how problems
are approached from a NoSQL perspective.  After all, how can I tell if I
have a problem suitable for a NoSQL database if the only way of thinking
I have is SQL?

I picked up the EPUB version of *CouchDB: The Definitive Guide* not with
the aim of using the product for any particular project, but to get a
sense of what the NoSQL movement is about, and how it changes the way
you think about problems.  The foreword from Damien Katz, the creator of
CouchDB, gave me confidence I had the right text: "The reason for this
book is that CouchDB is a very different way of approaching data
storage. A way that isn’t inherently better or worse than the ways
before—it’s just another tool, another way of thinking about things.
It’s missing some features you might be used to, but it’s gained some
abilities you’ve maybe never seen. Sometimes it’s an excellent fit for
your problems; sometimes it’s terrible".

CouchDB wants you to think in terms of documents.  Real-world,
self-contained, messy, documents.  If you're modelling an invoice, the
address for the invoice would be in the document, not a reference to
another document.   No mobile (cell) number for a particular invoice?
Fine, then leave it out of that document rather than being there as a
mandatory yet NULL field.  But if you get a number later, the document
can evolve to include it.  Storing arbitrary documents sounds like a
recipe for disaster, but this is avoided by supplying an optional
JavaScript validation function. The CouchDB server will reject documents
that fail validation.  

What you're left with is the benefits of simplicity: you can look at a
document and it makes sense by itself (or at least has that potential);
and the API for storing and querying documents is the REST model of PUT,
GET, DELETE.

In practice you run a CouchDB server, and insert documents represented
as JSON, making a document a wrapper around the usual JSON data types of
string, number, boolean, arrays and hash maps.   There are
language-specific APIs for this, but as the authors say "almost all
languages have HTTP and JSON libraries".  Many of the examples in the
book use cURL to show exactly what's happening when interacting with
CouchDB.

## Queries

Queries, called views in CouchDB, consist of a map function and an
optional reduce function, both implemented in JavaScript.  The map part
defines the set of information you're interested in.  It's applied to
each document in the database, and it can emit zero, one or more
key/value pairs.  Of course, this isn't run each time you make the
query.  What you're defining is an index, run once, and updated after
document changes, which produces a data set sorted by the key. This can
be combined with filters and ranges to select a subset of the results
from a view. 

The reduce function allows you to produce aggregate results such as
sums.  The weakest part of the book is the explanation of reduce and
this could be clearer.    However, if you've dabbled in functional
programming, some of this may make you feel at home: defining a mapping
function, filtering... it's not far from what you know.

The obvious question from SQL-land ("what about joins?") is answered by
way of an example of blog post comments.  The blog post and the blog
comments are separate documents, which is great for performance as there
are no locks when adding comments.  A query can return all posts and
comments, ordered by blog post ID.  You can then select from this view
just the post ID you want, which will give you the information you need
to show both the post and comments.  

What makes all this fast is the B-Tree implementation, and it's
reassuring to see an appendix and parts of the book dedicated to
explaining how the magic works under the covers.  I don't need to know
this, but it's great to see this in front of you, rather than just
hand-wave about what CouchDB is capable of.

## Beyond the basics

CouchDB doesn't stop at document storage and retrieval.  There are two
other key elements.  The first is that CouchDB can host  whole
applications.  You can define an application in terms of JavaScript
functions that live inside a CouchDB document (a "design document").
 These functions can produce HTML or whatever you need from inside
CouchDB, which is conceptually a nice fit with the RESTful and
JavaScript underpinnings of the server.  If you have database based on
web technologies it's not a stretch to host the whole web app.  It's
entirely optional, but I can see the attractions.  

Updating a design document each time you make an edit would be tedious,
but CouchDB offers a CouchApp tool to expand your application onto disk
for easy editing.  This gives you a layout that won't be too unfamiliar
to developers who use Rails or other convention-based frameworks. 

The second key feature is replication.  CouchDB supports replication
from one database to another, defined by a URL, meaning it can be local
or remote and it makes no difference to CouchDB.  An offline database
can use replication to bring itself up-to-date and transmit offline
modifications back to the mothership or peers. 

Conflict management is handled by a mandatory revision value on every
document. To make a change to a document, you need to provide the
revision number you're editing, and the server will reject a change if
you're not editing the latest revision.  During replication, however,
conflicts are flagged in the document, and an algorithm ensures that all
instances will pick the same document as the winning document and make
it the latest revision (it's based on the history size and the sorting
order of the document ID).   You can then find these documents and apply
your own conflict resolution and replace the winner.

You might expect this would make CouchDB great for implementing
something like infinite undo, but previous document versions are not
guaranteed to be retained.

These two features (documents as applications and replication) gives
CouchDB an interesting stance on application deployment.  An application
and the data can be replicated to clients, who can then fork the data
and application and optionally push it back.  Or you can lock it down.
 But an interesting angle, I feel, for app development.

## Likes and dislikes

What I'd like to see more of in this book are the idioms or patterns for
document thinking.  For example, there's a section describing how to
achieve document ordering in a to-do list application. If you want to
move an item up your to-do list, rather than change a position field on
every document to maintain an order,  you can set your document index to
be the floating point number which is the average of the positions above
and below your insertion point.  That has it's own problems, but is a
nice example of a different approach to problem solving.  There must be
more nuggets like that in the CouchDB community.

In a similar vein I'd love to see more on recipes for different
applications and an expanded "cookbook for SQL jockeys" section.

I've only touched on the basics: the book covers bulk updates, security,
load balancing, clustering, partioning, scalability (yes, the phrase
[web scale][] is used a couple of times), the API for watching
changes... there's plenty in here.  And throughout the book you'll find
hand-annotated code examples, which are a delight and comunicate a lot
in a little space.

I do have some gripes.  The practical side is where I hit my first
frustration with the book.  You're directed to "appendix D" to install
CouchDB, where you're told to avoid installing from source, followed by
the instructions for installing by source.  Google for "CouchOne" and
install that.   When the authors direct you to run the tests after an
install, they fail to mention they only work in Firefox.

There is some lovely writing on occasion, but the rough edges where the
sections don't flow or are lacking in introduction really stick out.  In
some places you're left feeling a little like a wiki page has been
dragged in to the book without enough blending.  The book would benefit
from an edit.

The technology is evolving, so there are inevitably mis-matches between
CouchDB 1.0 and the version used in the book (a 0.9 version).   In
particular, macros are out of favour and the templating format has
changed.  In fairness the authors point out it's the readers
responsibility to get the right versions of software and applications,
but it would have been great to have an easy-to-find download of the
exact version of the blog example codebase used in the book to follow
along.  But I still learned plenty from figuring out how the latest
version of the blog example worked.

But those are small complaints, and overall the book does a good job of
introducing the CouchDB way of thinking, and was a pleasure to read.

  [cover]: http://akamaicovers.oreilly.com/images/9780596155902/lrg.jpg
  [CouchDB: The Definitive Guide]: http://oreilly.com/catalog/9780596155896/
  [available online under a Creative Commons license.]: http://guide.couchdb.org/
  [web scale]: http://nosql.mypopescu.com/post/1016320617/mongodb-is-web-scale

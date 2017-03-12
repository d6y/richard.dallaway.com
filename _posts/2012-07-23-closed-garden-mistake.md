---
layout: post
comments: true
permalink: /:title.html
title: 'Closed Gardens: Don&#39;t repeat my mistakes'
author: Richard
date: 2012/07/23
tags:
---

Don't commit time and effort to a service you care about if it doesn't give you the ability to get your data out. A promise of an export feature isn't an export feature.

Posterous has been my learning experience on that front. Despite saying there would be instructions for exporting "over the coming weeks", it hasn't appeared, and meanwhile I've watched the [service rot](http://thenextweb.com/insider/2012/07/22/twitter-owned-posterous-loses-multiple-databases-service-down-for-2-hours/).

## Escape from Posterous

I ended up paying a few dollars to [Export My Posts](https://exportmyposts.jazzychad.net/) (which got most of my posts), then [scripted](https://gist.github.com/3092041) the oh-so-handy [Pandoc](http://johnmacfarlane.net/pandoc/) to do the grunt work of a HTML to Markdown conversion.  Followed by hours of clean up by hand.  But I'm in a happier place, and using [telegr.am](https://telegr.am/) for this site. The data can be in GitHub or Dropbox, in the format I write it in, and the rendering is all hosted: perfect.

_Update_: Scrub all of that stuff about export because [telegr.am natively imports from Posterous](https://blog.telegr.am/blog/migrate_from_posterous) now.

_Update Update_: Scrub that, I'm now using [Jekyll](http://jekyllrb.com/) to generate HTML, which is [synced to an S3 bucket](http://s3tools.org/s3cmd-sync).

## Escape from Flickr

Flickr is wonderful, unless you have comments and have annotated images: that stuff can't be exported. So I've abandoned Flickr for [OpenPhoto](http://theopenphotoproject.org/), which, although not as polished or feature-rich as Flickr (!) it has been created as "a cloud based open source photo sharing service where you own and control the photos, tags and comments". Since being [kickstarted](http://www.kickstarter.com/projects/jmathai/openphoto-a-photo-service-for-your-s3-or-dropbox-a) it has recently added Flickr import.

_Update_: OpenPhoto became Trovebox, which closed in 2015. I'm back to Flickr, reasoning the next replacement will be able to import from Flickr.

## What about Twitter?

Mitigation. I use [ThinkUp](http://thinkupapp.com/) to record all my Twitter interactions.

Good luck.

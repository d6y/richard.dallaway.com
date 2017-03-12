---
layout: post
comments: true
permalink: /:title.html
title: 'Comments for Logging'
author: Richard
date: 2007/07/05
alias: /comments-for-logging
tags:
---

Logging can become quite verbose. I found myself writing a method in
which most of the lines were logging related, which really clutters the
code:

	if (source == null)
	{
	 if (log.isDebugEnabled())
	 {
	   log.debug("No copy required as no source provided (dest was "+dest.getAbsolutePath()+")");
	 }
	 return;
	}


Possibly I'm over-logging, to try to avoid ever running a debugger. But
I was wondering if it'd be possible to do something so that I could turn
on *logging of comments* for a given method, class, package or whatever.
So if I'd written....

	if (source == null)
	{
	 // No need to copy as no source directory provided when copying to dest
	 return;
	}


...and there was some way to say "show me the comments as you execute",
that would be as useful as my four lines of debugging. You could even
imagine this hypothetical tool could spot references in comments to
variables in scope and do a `toString` on them.

This wouldn't replace logging (you'd still want error, info, fatal,
etc), but it might be a useful piece of ease-of-use. From what I can
tell Java doesn't preserve comments in the bytecode, so it's not
something easy to do. But we have annotations and reflection and AOP and
static analysis and assertions and all sorts of stuff, so there's
probably some way to make it happen.



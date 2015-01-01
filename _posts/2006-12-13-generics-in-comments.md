---
layout: post
comments: true
permalink: /:title
title: 'Generics in Comments'
author: Richard
date: 2006/12/13
alias: /generics-in-comments
tags:
---

For a project I'm involved in that is stuck using JDK 1.4, I've found
doing this soothing:
	
	 List/*<Stuff>*/ getStuff(); 

It's documentation, and gives me the hope that one day I can remove the
comments for this particular project.


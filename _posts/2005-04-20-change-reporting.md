---
layout: post
comments: true
permalink: /:title
title: 'Change reporting'
author: Richard
date: 2005/04/20
alias: /change-reporting
tags:
---

How to communicate code changes? I could send email, I could maybe read
some of the reports from [CruiseControl][], or I could look at
[Fisheye][] (or an open source alternative, such as
[CvsChangeLogBuilder][]).

FishEye is nice, but I don't find the need for most of what it can do.
What I do use is a text and RSS feed of CVS commit messages. Sure,
FishEye does that, but as that's all I want I'll save my US$999 and use
a [Perl script called cvs2cl][] that does the job.


    cvs co my-projectcd my-project
    # Text version
    cvs2cl.pl  --stdout -l "-d>1 month ago" > $OUT/changelog.txt
    
    # RSS version:cvs2cl.pl  --xml --stdout -l "-d>1 month ago" > $OUT/changelog.xml
    xsltproc  filter-cvs2cl.xslt $OUT/changelog.xml > $OUT/changelog.rss

Right now I have that running on the hour every hour, but it's probably
not too hard to plug it into a build tool or even have it run after a
[CVS commit][] event.


  [CruiseControl]: http://cruisecontrol.sourceforge.net/
  [Fisheye]: http://www.cenqua.com/fisheye/
  [CvsChangeLogBuilder]: http://cvschangelogb.sourceforge.net/
  [Perl script called cvs2cl]: http://www.red-bean.com/cvs2cl/
  [CVS commit]: http://www.pragmaticautomation.com/cgi-bin/pragauto.cgi/Monitor/LettingCVSPullTheTrigger.rdoc/style/print

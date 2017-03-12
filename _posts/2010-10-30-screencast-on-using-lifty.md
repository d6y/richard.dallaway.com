---
layout: post
comments: true
permalink: /:title.html
title: 'Screencast on using Lifty'
author: Richard
date: 2010/10/30
alias: /screencast-on-using-lifty
tags:
---

<iframe src="http://www.youtube.com/embed/kbwDibZmiRg?wmode=transparent" allowfullscreen="allowfullscreen" frameborder="0" height="417" width="500"> </iframe>

Lift development has moved on from the depressingly long Maven commands
to start a new project.  I made the above screencast to show the [simple build tool][] that I'm growing fond of, and an add-on for SBT called
[Lifty][] that makes it easy to create Lift projects.

It took me a couple of attempts to make the video, so of course many of
the automatically downloaded libraries were locally cached.  It'll take
longer for you if you're starting from scratch, but not as long as I'd
expected. For some reason (possibly imaginary) the SBT downloads for
Lift seemed much smaller, and consequently faster, than I'm used to with
Maven. YMMV.

If you want to know more about Lifty, check out [Mads Hartmann's talk][] from the Scala LiftOff London.  There's also an "[Up-close and personal with SBT][]" workshop from the London Scala User Group in the
evening on 10 November 2010 (free, just sign up).

**Going beyond the screencast**

I don't use SBT projects exactly as I've shown in the screencast: I use
JRebel and the [Scala IDE for Eclipse][].

"JRebel maps your project workspace directly to your running
application. When a developer makes a change to any class or resource in
their IDE the change is immediately reflected in the application,
skipping the build and redeploy phases".  And the Scala IDE compiles on
save, so the combination gives me "press save in the IDE, hit reload in
the browser" development.

I've paid for the enterprise version of JRebel as I have legacy Java
apps to live with, but for pure Scala/Lift development you can pick up
[a free Scala license of JRebel][] — and you should. 

The configuration is just in two places.  For SBT itself, modify the sbt
launch script to include the JRebel magic.  I have:

    export REBEL_HOME=/opt/zeroturnaround/jrebel
    export REBEL_JAR=$REBEL_HOME/jrebel.jar
    export REBEL_LIC=/opt/zeroturnaround/javarebel.lic
    export WITH_REBEL="-Drebel.license=$REBEL_LIC -noverify -javaagent:$REBEL_JAR"
    java $JAVA_OPTS -XX:MaxPermSize=256m -Xmx512M $WITH_REBEL -jar `dirname $0`/sbt-launch.jar "$@"

The other change is to prevent Jetty from restarting itself on change.
 That's an edit to your project/build/Project.scala file (or whatever
you called it):

    override def jettyWebappPath = webappPath
    override def scanDirectories = Nil

That's all, and when you fire up SBT and run the jetty command, you have
a pretty efficient working environment.

Oh, to generate the Eclipse project files, there are a few [options listed on the SBT wiki][], but I'm currently trying [SbtEclipsify][].

  [simple build tool]: http://code.google.com/p/simple-build-tool/
  [Lifty]: http://lifty.github.com/
  [Mads Hartmann's talk]: http://skillsmatter.com/podcast/scala/mads-hartmann-jensen-lifty-lightning-talk
  [Up-close and personal with SBT]: http://skillsmatter.com/event/scala/lsug-workshop-up-close-and-personal-with-sbt
  [Scala IDE for Eclipse]: http://www.scala-ide.org/
  [a free Scala license of JRebel]: http://sales.zeroturnaround.com/
  [options listed on the SBT wiki]: http://code.google.com/p/simple-build-tool/wiki/IntegrationSupport
  [SbtEclipsify]: http://github.com/musk/SbtEclipsify

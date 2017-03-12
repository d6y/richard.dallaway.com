---
layout: post
comments: true
permalink: /:title.html
title: 'SBT builds with Hudson'
author: Richard
date: 2011/01/14
alias: /sbt-builds-with-hudson
tags:
---

We build our Lift applications with Hudson [Jenkins][], and we're in the
process of converting some projects from Maven to SBT.  This is mostly
straight-forward but there are a couple of non-obvious things you might
need to know.

**Running SBT**

There's the start of [a Hudson SBT plugin][], but at the moment we're
running SBT from Hudson by simply shelling out to execute the SBT
command.

To do this, install SBT somewhere (`.hudson/tools/sbt` for us) and then
create a new free-style Hudson job and add a build step of "Execute
Shell".  We have:

    /usr/lib/jvm/java-6-sun/bin/java -Dsbt.log.noformat=true -jar /home/hudson/.hudson/tools/sbt/sbt-launch-0.7.4.jar clean update test package

That's it. Your project will build.

(The `-Dsbt.log.noformat=true` part turns off the SBT colour formatting
which you want off otherwise your build file will contain weird
formatting control characters).

**Controlling the WAR name**

By default the WAR filename will be something like
`foo-scala\_2.8.1-1.0.war`, which is fine, except because of the way we
deploy we wanted to drop the Scala version number.  That's easy to do,
and you can configure it in your SBT project file:

    override def artifactID = "foo"

**Adding build information to your MANIFEST.MF**

In our apps we like to include the Hudson build number in the footer of
every page. We do this by using the fact that Maven Hudson builds
include the build number in the `META-INF/MANIFEST` file.  SBT doesn't.

Fortunately, the fine Scala team at The Guardian have published [an SBT
plugin to add information to the MANIFEST][].  The install details are
in the README.  

Hudson sets the build number as an environment (shell) variable.  To
pick these up via the plugin we just set them as -D parameters:

    /usr/lib/jvm/java-6-sun/bin/java -DBUILD_NUMBER=$BUILD_NUMBER -DGIT_BRANCH=master -DJOB_NAME=$JOB_NAME -Dsbt.log.noformat=true -jar /home/hudson/.hudson/tools/sbt/sbt-launch-0.7.4.jar clean update test package

When that executes:

    /usr/lib/jvm/java-6-sun/bin/java -DBUILD_NUMBER=212 -DGIT_BRANCH=master -DJOB_NAME=FooHudsonJob  -Dsbt.log.noformat=true -jar /home/hudson/.hudson/tools/sbt/sbt-launch-0.7.4.jar clean update test package

Our SBT project file picks up these System properties:

    def revision = systemOptional[String]("GIT_BRANCH", "DEV").value
    def build = systemOptional[String]("BUILD_NUMBER", "DEV").value
    def versionName = systemOptional[String]("JOB_NAME", "DEV").value

The result is a WAR file with a `META-INF/MANIFEST.MF` file that looks
something like this:

    Manifest-Version: 1.0
    Revision: 
    Implementation-Title: foo-1.0
    Title: foo-1.0
    Implementation-Version: FooHudsonJob
    Branch: master
    Date: Fri Jan 07 14:41:51 UTC 2011 Build: 212

**Test results**

With what we have at the moment if a test fails, the build fails.  This
is good.

What we don't have is the nice graph of tests.  If you want that, take a
look at  Christoph Henkelmann's Blog:  [How to build sbt projects with Hudson and integrate test results][].

For us, the next step is adding in [the Undercover code coverage
reporting tool][] instead. 

  [Jenkins]: http://jenkins-ci.org/
  [a Hudson SBT plugin]: https://github.com/hudson/sbt-plugin
  [an SBT plugin to add information to the MANIFEST]: https://github.com/guardian/sbt-war-manifest-plugin
  [How to build sbt projects with Hudson and integrate test results]: http://henkelmann.eu/2010/11/14/sbt_hudson_with_test_integration
  [the Undercover code coverage reporting tool]: https://github.com/NumberFour/sbt-undercover-plugin

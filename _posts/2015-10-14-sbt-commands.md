---
layout: post
title: SBT Tricks
author: Richard Dallaway
---

Dear Richard: these are the SBT tricks you should have known about.
They are the ones you wish you could remember just when you need them, but you don't.
So here they are written down.

<!-- break -->

# Changing JVM Options

Mid-session you sometimes want to change JVM flags.
The `javaOptions` setting is good for that. For example..

## Trace Typesafe Config Files

~~~
project foo
set javaOptions := Seq("-Dconfig.trace=loads")
run
~~~

## Cheap and Cheerful Profiling

Nothing like as good as Mission Control or JProfiler, but still...

~~~
project foo
set fork in run := true
set javaOptions in run += "-agentlib:hprof=cpu=samples"
runMain code.MyMain
~~~

...and then look in _foo/java.hprof.txt_.


# Triggered Execution

You usually prefer `~test:compile` over `~compile` when writing code.

You always want the console cleaned at the the start of each run:

~~~
triggeredMessage in ThisBuild := Watched.clearWhenTriggered
~~~

You sometimes need `test:run` or `test:runMain` when that important application is in _src/test/scala_.


# Stopping

When you run an application and CTRL-C it, it's annoying if that exits sbt.
Prevent that with:

~~~
cancelable in Global := true
~~~

# Latest Dependencies

Ivy dependencies allow you to select “latest.integration” as the revision number:

~~~
addSbtPlugin("org.ensime" % "ensime-sbt" % "latest.integration")
~~~

This will give you the latest and greatest version. Use it for global plugins (see below) for you development environment. Avoid it for core build dependencies because you want a reproducible build at all times.

#  The Place for Everything

* _~/.sbt/0.13/plugins/_ folder for the plugins you use everywhere, such as ensime-sbt.

*  _~/.sbt/0.13/global.sbt_ for settings relating to those plugins.

* _~/.sbtrc_ for commands you expect to run.

* _/usr/local/etc/sbtopts_ exists, and you hope to never have to touch it.

For example:

    $ cat ~/.sbt/0.13/plugins/build.sbt
    addSbtPlugin("net.virtual-void" % "sbt-dependency-graph" % "latest.integration")
    addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "4.0.0")
    addSbtPlugin("com.github.mpeltonen" % "sbt-idea" % "latest.integration")
    addSbtPlugin("org.ensime" % "ensime-sbt" % "latest.integration")

    $ cat ~/.sbt/0.13/global.sbt
    net.virtualvoid.sbt.graph.Plugin.graphSettings
    triggeredMessage in ThisBuild := Watched.clearWhenTriggered
    cancelable in Global := true

    $ cat ~/.sbtrc
    alias cd = project

Although you typically shovel everything into _build.sbt_, sbt will read all the _.sbt_ files in those folders.

# Defining Commands

E.g.,

~~~
$ cat ~/.sbt/0.13/clear.sbt
// Handy `clear` command:
def clearConsoleCommand = Command.command("clear") { state =>
  val cr = new jline.console.ConsoleReader()
  cr.clearScreen
  state
}

commands += clearConsoleCommand
~~~

# Problem Solving

It's always something to do with scopes.

You can find the value of a setting in a particular scope using inspect:

~~~
inspect project1/libraryDependencies
inspect project2/libraryDependencies
~~~

So sometimes you want to make a setting the same for all projects in a build:

~~~
scalaVersion in ThisBuild := "2.11.7"
~~~

You should regularly re-read the [scoping rules](http://www.scala-sbt.org/release/tutorial/Scopes.html).

# Other Ideas

A version of this post appears over [at Underscore.io](http://underscore.io/blog/posts/2015/11/09/sbt-commands.html).
It includes comments and other tips submitted by the community.




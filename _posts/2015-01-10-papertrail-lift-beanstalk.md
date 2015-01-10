---
layout: post
comments: true
permalink: /:title
title: Papertrail Logging with Lift and AWS Beanstalk
author: Richard
date: 2015/01/10
tags:
- scala
- lift
- aws
- logging
---

[Papertrail](https://papertrailapp.com/) is a web-based log management tool. It aggregates log messages from your applications, allowing you to  view and search them from a web console. It's web-based `tail -f` and `grep` in one.

Setting up Papertrail with Lift on AWS isn't hard, but there are a couple of steps that I didn't think were obvious. You'll find the steps listed out below.

The title of this posts talks of Beanstalk and Lift, but much of what is here is probably applicable to any JVM framework, raw EC2 deployments, and possibly other cloud hosting.

At the top level there are four steps:

1. Create and configure Papertrail _log destination_.
2. Optionally set the log destination port and host as configuration parameters in AWS.
3. Configure logback to use the Papertrail appender.
4. Add the Papertrail Logback appender as a dependency in your build.

## 1. Create a log destination

You do this in the Papertrail web interface, from the "Account" tab.  You want to enable the settings to:

- Yes, recognize logs from new systems
- Accept connections via TCP, plain text.

The result of this will be a hostname and port. E.g., _logs2.papertrailapp.com:12345_.

You'll notice that we're setting up plain text TCP logging. The options below cater for that specific situation. If you want to vary that configuration, you'll need to vary what's below too.

## 2. Store the host and port in AWS

Rather than hard code the Papertrail configuration into an application, I prefer to set this in Beanstalk as system properties. You define these in the _Software Configuration_ section of your Beanstalk environment.  I define two properties:

- `PAPERTRAIL_HOST`, which is the hostname you were given when creating the log destination.
- `PAPERTRAIL_PORT`, the matching port number

## 3. Configure logback

We can now reference those host and port values in our logging configuration.  For Lift, this would be _src/main/resources/props/production.default.logback.xml_ or similar:

<script src="https://gist.github.com/d6y/c98d784880cc42544c82.js"></script>

You'll notice we can use `${PAPERTRAIL_HOST}` in the XML and that'll work out just fine.  Obviously, if you didn't set these properties up you can just paste in your host and port values into the XML.

## 4. Appender dependency

The final part to make this work is to depend on the [appender](https://github.com/papertrail/logback-syslog4j):

    "com.papertrailapp" % "logback-syslog4j" % "1.0.0"

...that goes in your _build.sbt_ in the `libraryDependencies`.

## And you're done

Deploy that, and you'll have your log entries appearing over in the Papertrail events log.

The [Lift Cookbook](http://chimera.labs.oreilly.com/books/1234000000030) talks a little bit more about Lift and deployments, including to Beanstalk.


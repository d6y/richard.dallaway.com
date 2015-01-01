---
layout: post
comments: true
permalink: /:title
title: 'Watching Production Logs'
author: Richard
date: 2007/10/22
alias: /watching-production-logs
tags:
---

I've always believed that production application error logs should be
empty. I've also implicitly assumed that any errors in live application
logs are rare, freakish, occurrences, and on the whole of little
relevance. But... what would it take to get an empty error log? Or at
least an error log that only contained truly anomalous events?

You know what I mean, right? You sprinkle your code with
`log.error("a bad thing happened", exception)`, and during development
and testing maybe you see them. On your production machines you might be
lucky enough to have someone who knows and watches the logs, or perhaps
you occasionally look at the logs yourself: but on the whole they are
ignored unless someone starts jumping up and down—and then you take a
look at the logs.

Well, that's going to change for me. I've now become addicted to having
production application error messages sent to me by email (before you
offer, no thanks: you can keep your own errors to yourself). It's quite
instructive to see messages in your mailbox the moment something odd
happens in production.

Here's an trivial example of what you might see:

	From: whoever
	Subject: [SMTPAppender] richard@toto error message
	Date: 19 October 2007 19:43:10 BDT
	To: someone

	[2007-10-19 19:43:10,290]

	ERROR

	Test

	Caught an exception

	java.lang.NumberFormatException: For input string: "Hello world"
	 at java.lang.NumberFormatException.forInputString
	 (NumberFormatException.java:48)
	 at java.lang.Integer.parseInt(Integer.java:447)
	 at java.lang.Integer.parseInt(Integer.java:497)
	 at Test.main(Test.java:30)

The kinds of things we've seen so far are:

-   A couple of spiders hitting URLs that that don't exist, but must
once have been incorrectly published.
-   A user calling an action but not passing any parameters. I'm
thinking they've bookmarked something that ought to be bookmarkable,
but isn't (what can I say: we didn't write all the code that we
support).
-   A plain good-old straight-forward bug in some background task that
no-one would ever know was failing, but is now going to get fixed.


I'm not telling you anything new here. I'm just saying that it's
worthwhile trying.

Setting this up with log4j is pretty straightforward. There's a good
article over at [ONJava.com][] that explains it. All you need to do is
configure the appender in log4j.xml (if you're using the .properties
version, migrate to the .xml version first). Here's an example:


	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
	<log4j:configuration>
	 <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
	   <param name="Target" value="System.out" />
	   <layout class="org.apache.log4j.PatternLayout">
	     <param name="ConversionPattern" value="[%d{ISO8601}] %-5p %c %m %n" >
	   </layout>
	   <filter class="org.apache.log4j.varia.LevelRangeFilter">
		 <param name="LevelMin" value="DEBUG"/>
		 <param name="LevelMax" value="INFO"/>
	   </filter>
	  </appender>
	  <appender name="STDERR" class="org.apache.log4j.ConsoleAppender">
		<param name="Target" value="System.err" />
		<layout class="org.apache.log4j.PatternLayout">
		  <param name="ConversionPattern" value="[%d{ISO8601}] %-5p %c %m %n" />
		</layout>
		<filter class="org.apache.log4j.varia.LevelRangeFilter">
		  <param name="LevelMin" value="ERROR"/>
		  <param name="LevelMax" value="FATAL"/>
		</filter>
	  </appender>

	<appender name="email" class="org.apache.log4j.net.SMTPAppender">
	 <param name="BufferSize" value="10" />
	 <param name="SMTPHost" value="your host here" />
	 <param name="SMTPUsername" value="optional username" />
	 <param name="SMTPPassword" value="password if you need it" />
	 <param name="From" value="whoever@blah" />
	 <param name="To" value="someone@blah" />
	 <param name="Subject" value="[SMTPAppender] ${user.name}@${hostname} error message" />
	 <layout class="org.apache.log4j.PatternLayout">
	   <param name="ConversionPattern" value="[%d{ISO8601}]%n%n%-5p%n%n%c%n%n%m%n%n" />
	 </layout>
	 <filter class="org.apache.log4j.varia.LevelRangeFilter">
	   <param name="LevelMin" value="ERROR"/>
	   <param name="LevelMax" value="FATAL"/>
	 </filter>
	</appender>

	<root>
	 <level value="all" />
	 <appender-ref ref="STDOUT"/>
	 <appender-ref ref="STDERR"/>
	 <appender-ref ref="email"/>
	 </root>
	</log4j:configuration>


Yeah, yuck to look at, but it's pretty simple. The part you want is the
`SMTPAppender` bit in the middle. Leave out the username and password
param tags if your SMTP server doesn't require authentication.

The XML above is almost the exact example from the ONJava.com article,
except for a couple of items I needed. The first is that I wanted the
name of the user the application was running under, and also the
hostname of the box the application was running on. To do that, I've
used a feature the fine log4j people included: environment properties
substitution. You'll see `${user.name}` in the subject line, which is a
standard [Java system property][]. The `${hostname}` is a property I
provided to the application by starting it with
`` -Dhostname=`uname -n` ``.

If you want to you can also dynamically modify the SMTP Appender at run
time by iterating over the root logger, checking for an instance of
SMTPAppender and then changing what you want. If you do that, though,
the clue you need is to call `appender.activateOptions()` when you're
done modifying the appender.


  [ONJava.com]: http://www.onjava.com/pub/a/onjava/2004/09/29/smtp-logging.html
  [Java system property]: http://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()

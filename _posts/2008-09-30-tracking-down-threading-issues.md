---
layout: post
comments: true
permalink: /:title.html
title: 'Tracking down threading issues'
author: Richard
date: 2008/09/30
alias: /tracking-down-threading-issues
tags:
---

One of the projects we've been working on involves kicking off a number
of threads to check on availability of certain services. Despite having
read the fantastic [Java Concurrency in Practice][] book (which you
should buy, possibly [from Amazon UK][]) we'd run into a situation where
Tomcat wasn't shutting down. Inevitably it'd be because we'd starting a
thread that wasn't stopping. As a reminder to myself here are a bunch of
useful tools for figuring out what might be going on....

Step 1: find out what's running with `jps -lm`:


    $ jps -lm
    1001 org.apache.catalina.startup.Bootstrap start
    1302 sun.tools.jps.Jps -lm
    1066 com.j_spaces.core.client.SpaceFinder /./taykt
    176 


This is a list of the Java process IDs which are "typically, but not
necessarily, the operating system's process identifier for the JVM
process", according to the [JPS manual page][]. In my case, they were
the OS process IDs, although I should point out that I'm running this on
Mac OS 10.5 using JDK 5, and the tools do vary by operating system.

What the list is showing me is that the Tomcat process is still running
(process 1001). All the other processes I expect to see on my machine,
including the mysterious 176 which happens to be IntelliJ.

Step 2: take a look at what threads are running in the process, as
described in the [J2SE 5.0 Trouble-Shooting and Diagnostic Guide
PDF][]:


    $ kill -QUIT 1001 


Hmm. No output from that command... of course, because I've signalled to
the Tomcat process to give me a thread dump the output will be in the
Tomcat logs:


	==> catalina.out Full thread dump Java HotSpot(TM) Client VM
	(1.5.0\_16-133 
	mixed mode, sharing):
	
	"DestroyJavaVM" prio=5 tid=0x010017f0 nid=0xb0801000 waiting 
	 on condition [0x00000000..0xb0800060]
	
	"SelfCleaningTable\$Cleaner" daemon prio=5 tid=0x01049030 
	 nid=0x8e7a00 in Object.wait() [0xb171f000..0xb171fd90]
	 at java.lang.Object.wait(Native Method)
	 - waiting on (a java.lang.ref.ReferenceQueue\$Lock)
	 at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:120)
	 - locked (a java.lang.ref.ReferenceQueue\$Lock)
	 at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:136)
	 at com.j\_spaces.obf.eb.run(SourceFile:231)
	
	"pool-1-thread-1" prio=5 tid=0x01021470 nid=0x8f0c00 waiting
	 on condition [0xb0c8a000..0xb0c8ad90]
	 at sun.misc.Unsafe.park(Native Method)
	 at java.util.concurrent.locks.LockSupport
	 .park(LockSupport.java:118)
	 at java.util.concurrent.locks.AbstractQueuedSynchronizer$
	 ConditionObject.await(AbstractQueuedSynchronizer.java:1841)
	 at java.util.concurrent.LinkedBlockingQueue
	 .take(LinkedBlockingQueue.java:359)
	 at java.util.concurrent.ThreadPoolExecutor
	 .getTask(ThreadPoolExecutor.java:470)
	 at java.util.concurrent.ThreadPoolExecutor$Worker
	 .run(ThreadPoolExecutor.java:674)
	 at java.lang.Thread.run(Thread.java:613)
	...


...it goes on for a bit. The parts I was interested in were non-daemon
processes (because daemon ones are shutdown when the JVM shutsdown). In
my case, the "pool-1-thread-1" process reminded me that we'd put a few
things into an [Executor][] and possibly not cleaned up properly. I know
it was that thread name, because I'd seen it in the application log
files. A quick dip into Java Concurrency in Practice to remind myself
about The Rules, and I had a few modifications that resolved the issue.
But...

Step 3: try out [JConsole][]. The trick here is that you need to set a
flag at Tomcat start up time:


    $ export CATALINA_OPTS="-Dcom.sun.management.jmxremote"
    $ sh startup.sh


Then a quick `jps -lm` to find the process number, then fire up JConsole
with `jconsole pid-goes-here` and you're set. Not only do you get easier
access to the thread list, you also have buttons in the MBean tab with
tempting names.


  [Java Concurrency in Practice]: http://www.javaconcurrencyinpractice.com/
  [from Amazon UK]: https://www.amazon.co.uk/dp/0321349601?tag=richarddallaway&camp=1406&creative=6394&linkCode=as1&creativeASIN=0321349601&adid=1WQS7WWCW5S75PR2MJNY
  [JPS manual page]: http://java.sun.com/j2se/1.5.0/docs/tooldocs/share/jps.html
  [J2SE 5.0 Trouble-Shooting and Diagnostic Guide PDF]: http://java.sun.com/j2se/1.5/pdf/jdk50_ts_guide.pdf
  [Executor]: http://java.sun.com/j2se/1.5.0/docs/api/java/util/concurrent/Executor.html
  [JConsole]: http://java.sun.com/developer/technicalArticles/J2SE/jconsole.html


---
layout: post
comments: true
permalink: /:title.html
title: 'Amazon Simple Queue Service'
author: Richard
date: 2005/04/27
alias: /amazon-simple-queue-service
tags:
---

Some months back I received email from Amazon announcing the beta launch
of their [queue service][]. I've just checked, and it's still in beta.

The idea is that you can create a queue on Amazon servers and then place
messages in the queue and consume messages from the queue. Messaging has
been around for quite a while, so what's interesting here? Well, what we
have is a company that has invested in an impressive (I assume!) network
and hardware infrastructure for the sale of books, and on the back of
this they are offering a software service -- and now I get to piggy back
on the level of service offered by their investment.

So let's say I, as [a company][], want to build some new service.
Pretend I'm going to accept photographs from customers and turn them
into something... print them or put a humorous frame around them....
whatever. I don't have to go out and spend money on a hardware platform
to build this on. I can use Amazon's reliability and scalability to
accept customer orders (messages going into the queue), and use whatever
infrastructure I have to consume orders from the queue as and when I'm
able to. I can offer 24x7, 99.999% uptime for customers without the
level of investment that this usually implies. That's interesting.

As I said, it's in beta at the moment, so it's free. It's REST and SOAP
based, it'll store up to 4,000 4k messages for 30 days, and there's a
critique over at [XML.com][]. I can't find any published SLAs or
pricing, but presumably they will follow.

Amazon are not the only company with a hardware infrastructure to use in
new ways. The obvious other examples are Yahoo! and Google. It'll be a
good sign if any other players decide to offer up these kinds of
services.

**Update 14 July 2006:** [Amazon SQS][] has now [gone into production][], with a cost of "10 cents for a 1000 messages, and 20
cents per Gigabyte of data transferred". No up-front costs, minimum
usage or subscription fees. This is astonishingly good, and one of the
smartest offerings I've seen from any company.


  [queue service]: http://www.amazon.com/gp/browse.html/ref=sc_fe_l_1/102-3465464-4496947?%5Fencoding=UTF8&node=13584001&no=13584171&me=A36L942TSJ2AJA
  [a company]: http://www.spiralarm.com
  [XML.com]: http://www.xml.com/lpt/a/2005/01/05/restful.html
  [Amazon SQS]: http://aws.amazon.com/sqs
  [gone into production]: http://www.allthingsdistributed.com/2006/07/your_queues_are_ready.html

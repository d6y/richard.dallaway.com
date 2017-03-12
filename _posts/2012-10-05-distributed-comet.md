---
layout: post
comments: true
permalink: /:title.html
title: 'Distributed Comet with Lift and Amazon SNS'
author: Richard
date: 2012/10/05
tags:
---

We've published a [Lift module to allow you to talk to Amazon SNS](https://github.com/SpiralArm/liftmodules-aws-sns/).  [SNS](http://aws.amazon.com/sns/) is Amazon's pub/sub system, and the module lets you register a function to receive messages from an SNS topic, and send messages to SNS.

The module just gives you the plumbing to send and receive messages, the config is in the project's README. 

Here's the kind of thing we use it for:

<iframe src="https://docs.google.com/presentation/embed?id=1tt4dTwJjAoIAoDM1ML-8tkEryduO2xp3pl5kdDUiQgk&start=false&loop=false&delayms=10000" frameborder="0" width="480" height="389" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

This is all pretty similar to Diego's approach to [a distributed Lift Comet Chat Application](https://fmpwizard.telegr.am/blog/distributed-comet-chat-lift).

We don't actually use it for chat, but [multi-user chat in Lift](http://simply.liftweb.net/index-Chapter-2.html) is an easy example to discuss.

**Wiring SNS into a comet application**

Lift handles the comet plumbing for us, and all we need to do is send a message to an Actor for all of that to be taken care of. But as the slides above indicate, we need to jump in an have those messages go out to SNS.  This is how we do that.

We enhance `LiftActor` with a new method which we call `>!`:

<script src="https://gist.github.com/3844746.js"> </script>

When we say `ChatServer >! Chat("hi")` you can see that if SNS is defined, we send the message to the SNS service; otherwise we deliver it as if the call had been made with `!`.

The implicit argument in that code means, whenever we call `>! Chat`, we're making the compiler find the class that can serialise that `Chat`.  Consequently, we can't accidentally send to SNS something we don't know how to serialise, because if we tried that, the code wouldn't compile.

We do this for a bunch of reasons, one of which control over what is sent, but another is to avoid the message (`Chat`) having to know anything about the serialisation mechanism. It's nothing to do with that class, and we might change out to a different pub/sub backend, and the `Chat` class shouldn't care at all.

This serialisation part is the type class pattern. I may be biased, but I recommend [Underscore](http://underscoreconsulting.com)'s [@channingwalton](https://twitter.com/channingwalton)'s [example of the type class pattern in Scala](http://www.casualmiracles.com/2012/05/03/a-small-example-of-the-typeclass-pattern-in-scala/).

Here's how it applies to us:

<script src="https://gist.github.com/3844759.js"> </script>

We're serialising as JSON, and for Chat there's really not a lot to pass other than the text of the message.  Telling the compile the `ChatFormat` object is implicit means it can be used to satisfy the `SNSSerializer` requirement on our `>!` call.

The final part is handling the message back from SNS:

<script src="https://gist.github.com/3844774.js"> </script>

That's the function we register with SNS during `Boot`.  It's a bit annoying that the handler duplicates the actor call in your Lift App (the `ChatServer ! Chat(msg)` part), but otherwise it all works out OK.

Hope that's of some use if you're running with N > 1 servers.



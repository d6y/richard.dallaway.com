---
layout: post
title: "Day in the Life of a Functional Programmer"
author: "Richard Dallaway"
---

Earlier this week I gave a 45 minute talk on "A day in the life of a functional programing".
The title is one Miles came up with when we were brainstorming the talk.
The idea was to try to get across some of the exciting ideas in FP to non-Scala developers.
We went for ADTs and type classes.

Notes what I said (or probably said) are in this post, along with slides.


<!-- break -->


<script async class="speakerdeck-embed" data-id="6f2f34eb32ce4c24b61b07026837d393" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

_You might want to [open the slides](https://speakerdeck.com/d6y/day-in-the-life-of-a-functional-programmer) in a separate window to click along._

## Modern development ##

Hello.

We're here to talk about modern software development. 
And the way we're going to do that is to give you a day in the life of a functional programmer.

We'll go through the kinds of things you do writing software,
but through the eyes of a functional programmer.
How a functional programmer might see problems, and approach a solution.

And we're doing this because for us modern means functional programming with types.

## Functional programming with types ##

And we'll get on to what that means in a moment.
But this is a  powerful, exciting, and liberating style of development.

Notice, that we’re not talking about being modern because of any particular product or architecture.
We're not modern because we're using React, or MongoDB,  or whatever is hot right now.

Rather, it's the ideas and techniques from functional programming that matter.
They give software developers a huge helping hand in every-day programming.

## Why now ##

FP is about programming using functions,
and using functions as values, passed to other function,
recursion,
lots of ideas stolen from maths.

We especially like functions that produce no side effects—something we’ll get to later.
Because they are easer to reason about, and it’s been realised they are great in a concurrent programming.

But these core ideas have been around for decades.
So why are we excited about this now?

## Modern compilers ##

It's because of modern compilers have powerful type systems.

We're familiar with compilers and languages that catch errors for us.
You gave me a string but I wanted an integer kind of errors.

That's great, but we can do a lot more than that.
We can get the compiler to do some of our work for us.
Which if you think about it, is kind of handy.



## A Day in the life ##

So…a day in the life. 
We’re going to see code. 
Quite a bit of code.
In three examples.
It’s not crucial that you follow every line.
I just want to get some ideas across as concretely as I can.



## Take away points ##

As I go through this there are a couple of things I hope you’ll spot.

The first is that functional programming is for everyone.
Some of the ideas may be new to you.
They become natural with practice.
But they have unfamiliar names even though they are simple.

The second thing we want you to watch out for is that these techniques make change easier.
And it does that by making your code better modularized and easier to reason about.

----

# 1. ADTs #

## Morning stand up ##

So let’s start our day.
Like any other, we’ll have a daily stand up to discuss what we’re working on.

The example we’re using is that the team has been building a web service.

It's a JSON based API behind a customer management system.

This is all asynchronous, but basically a pretty simple CRUD-style application. Actually, what it’s storing and how it stores it doesn’t much matter for the discussion today.


## REST API ##

All we have at the moment is customers. 
If you call the api, like this, it’ll give you back a lump of JSON with some customers in it. And a customer has an id, a name, and a phone number.

Simple stuff.


## New ticket ##

We've picked up a new ticket, and that's to add a subscription indicator to a customer.
Sounds like we're adding a field to a record.
Not too tricky.

## More details ##

We dig a little deeper and find out that a customer might have:

- No subscription
- An Active Subscription
- Lapsed Subscription

We're going to have to show that in JSON,
store it in the database,
write a state machine to manage transitions between the states.

How could we do that? Instantly a functional programmer says:

This is a job for an....Algebraic Datatype
These might not be phrases you’re familiar with, but it’s a simple and flexible way to model data. In fact, you'll wonder what the fuss was about.

Let’s work through what this means.


## Code ##

We'll see some Scala syntax now.
We're not here to teach you the language this afternoon.
You're all smart people, you'll get the general idea.
And that’s all I want to do is give you a sense for problems get solved.

We'll give you the code so you can explore it in more detail if you want to. 

We know we need something called a subscription.
We define it like this.
A trait is like an interface. 
An important part of this is that it is sealed,
which means "hey compiler, you're about to see all the possible kinds of Subscription"

And now we can list them out:

We're saying that Never is a case of Subscription.
We’re saying it’s an object, which just means there’s only one instance of never in our system.
And we’ve done the same for Active, and Lapsed.
And those are only the kinds of subscription.


We might also expect that some of these subscriptions have additional attributes.
Perhaps we want to know when an active subscription is due to expire:
Which is fine, each case can have different relevant fields. Then there could be many different active subscriptions.

Finally, we'd include a subscription in our representation of a customer.

And that’s our representation sort out.
A customer has a name, phone number, subscription,
And a subscription can be any of these three things.

That’s our algebraic data type. Everyone ok with that?

What do we need to do next?
Well, we could look at the database, or the JSON representation,
but we're going to talk about them later.

So let's instead look at a state machine for managing subscriptions.
We’re going to take a subscription and some date, and do something to produce a new subscription.

This is the syntax for a method in Scala.
We take two parameters, and we’re going to get back a subscription.
To complete this we fill out the question marks with some code.

We want to maybe handle expiring of subscriptions in some way.
Because this is an ADT we don't have to think much about it.
We know we have to handle each case in turn

What we're saying here is if the subscription matches Lapsed or Never,
we have nothing to do and we return the subscription.  That’s the value to the right of that fat arrow.

For an active subscription we need to do something.
Well, we match out the date, and I’ve called it exp here.
And we test to see if the expiry date has passed.
If it has, the subscription is Lapsed; otherwise it’s unchanged.

Everyone happy with what’s happening here?

So we have a function that we can run with a subscription at some time
and we get back a subscription.
Pure function, no side effects here.
We have separated the description from the behaviour.

Let’s see what this gives us.




## Change ##

Inevitably, something will change.

We have another case for a subscription.
That’s a simple change.
So we go back to our code.
And what we need to do is add in a case.

We can do that easily enough, 
but if we don't change our code in this transition method,
it won't compile any more.

We’ll be told there’s an non-exhaustive match.
Our method has covered only three of the four possible cases.
We need to go and fix up each place where we have some logic about a subscription.

And that's really nice, because we can mess with our data type, 
and have the compiler point out each and ever situation we need to re-examine.
It’s also really nice because we can do arbitrary things with our subscription. We don’t have to try and show horn all possible behaviours into once place.

BTW, this might look like a feature of Scala.
That we can match and represented sealed cases.
But it's a feature common to many functional programming languages.
Which is to say, it's a technique that has been around a while,
and works for people.

## Versatility ##

Exhaustivity check is one reason we like ADTs and SR.
Another reason is the versatility.
It’s a pattern we can use over and over.

For example, the code we have so for is okay.
Really, we probably want to do things.

Like maybe send a nagging email if the subscriber has lapsed.
Or send an incentive if the customer is expiring soon.

So what does the functional programmer do about that?
What we're not going to do is mix up side effects into this code. 
We’re not going to add things that change the world, like sending email,  in here with our business logic,
Instead we isolate the side effects, and move them as far out as we can to the edges of our code.

We can do that with another ADT

We can change our transition to take a customer, because maybe we want their subscription and their email address,
and produce some kind of action. 

What would an action be? An ADT.
Maybe there's no action. No change to be made.
Or maybe we want to send them some kind of encouragement to renew.
Whatever we want to do.

Transition itself would still be a pure-function.
I'm not going to fill the details of the code in.
It's the same kind of thing you've already seen: match of the cases, produces some value.
And you can still unit test that as much as you like.
Because it doesn’t change the state of the world.

When it comes to sending the email, or text or whatever, 
we can separate that out.
A method perform takes an action and returns Unit.
Unit is Scala's representation for no value.
It's signalling there's a side effect going on.

The body of perform would have to match on all the kinds of action.
Again, I won't bore you with the details.
It's the same as you've already seen.

Which is kind of the nice thing about ADTs.
You do a bit of up-front thinking about the various cases.
Action, Subscription.
Then writing the code to handle them is something you can just hammer out on the keyboard,
knowing the compiler has your back if you miss a case.

## Coffee and reflection ##

So far in our day in the life, we've done some work on the ticket.
We've written maybe twenty lines of code, so it's definitely time for some coffee and cake.

And on reflection, we can say that ADTs and structural recursion is one technique a functional programmer uses.
And they'll use it every day.

We like ADTs for their safety. How the compile helps us out.

We like them for their versatility.
They occur everywhere. If you've heard of Option or Optional types,
that's an ADT with two cases: Some and None.
Lists are ADTs, again with two cases: 
the empty list, and a "cons", which is a value and another list.
And you can pattern match on them.
So ADTs are fundamental building blocks, as well as useful in modelling our domain.

Separating a representation from doing it
Helps isolate side effects
...makes change easier

----

# 2. Type Classes #

## Back to work ##

Let’s progress this ticket a bit more.
The next step is that we want the subscription to appear in our JSON.  Something like this.

The punchline here is that we don't have to do anything.
This is a problem that’s solved already with a variety of libraries in Scala, which can figure this out for us.
I’m not going to show you how to use a library, instead I want to illustrate the principle behind it.
Because it's another pattern that appears again and again.
So we'll recreate the basics here.

And this pattern is referred to a type classes.
In particular, we create tiny building blocks,
and combine them up into bigger things.

This is important, because writing small things tends to be relatively easy, which makes development easier.

## JSON ##

Let’s say we have a value we want to turn into a valid JSON we can send back to a web browser.

We end up with a text representation to send down the wire.
In the case of an integer, it's pretty easy: toString.

If I have an integer in my programming language, I can turn it into text with toString.
It doesn’t look any different here, but that second 7 is now text.

In the case of a string, it's similar but we need quote marks.
So we take a string like Hello in our programming language, and stick quotes around it. That’s now text we can send to a browser.

That will let us create structures like this. Where the Hello is correctly quoted, and a number like count is as it should be, without quote marks.

In both these cases we take a value and return a string representation.  That's a function.
And in general we want to be able to go from any type, let’s call it T, to a String.

## Encoding an Idea ##

Let's encode that and give it a name.
Here’s another interface, we’re calling it a JsonFormat.
This is all abstract at the moment.
We can’t do anything with this.
It just says to be a JSON format for a value of some type, you have to have a method called format.  Format will take a value of some type, and give us back a string.

Let's create some examples of it.
This is the code we’ve already seen, but packaged up into JsonFormat.

Now, we can write a general purpose formatter.
So I’ve called this outputJson.  You give it a value of some type…  and you also give it a formatter for that type.
In Scala you can have multiple parameter blocks. You’ll see why that’s nice in a moment.

Now this looks a bit ... pointless. To output json, you give me a value, and a formatter for that value, and we format it. Doesn’t look like it’s doing much.

We do this, because in Scala we can ask the compiler to prove it can format something.

## Compile working for us ##

The code is now going to change very slightly. There are four changes, and I’ll point them out.

First, notice the the outputJson second parameter is marked implicit.
This keyboard gives the compiler permission to fill this in for us if we omit it.

It can only use values we’ve also marked as implicit.
So that’s another two changes: intFormat and strFormat are marked as implicit too.
That tells the compiler it can use these values to fill in parameters.

This means we can now call outputJson with just a value. When the compiler sees this it says. Ok, outputJSON has been called with an Integer.  But the lazy programmer hasn’t told me how to format that. But I’m allowed to fill that in…. Is there a JsonFormat that matches T, where T is an Int? Yes there is, so I can use that.

So now we’ve got something really powerful.
What makes this powerful, is that we can use this to build bigger structures from small pieces of code.

Let’s see that in action.

## Big things from little pieces ##

How about we want to convert a list of strings to Json.
Or a list of integers.
Or a list of list of integers.
We don't want to have to write a list of integer formatter,
And then a list of string formatter.
And then a list of list of integer formatter. 

We want to write a list formatter once and have it work with anything.
How do we do that?

## Formatter for lists ##

We start just the same way we did for formatting integers or strings.
We create a JsonForamt for a list of something.
And so we have to define a format method.
It takes a list of things, and gives us back a String … the JSON representation of that list.

Let’s fill in the body.
We know we need some square brackets around our values, and the values need to be separated by commas.

Where do we get the formatted values from?
Let’s fill in that code.

We can use this for structure in Scala…. For each value in our list, we’re going to do something.  Yielding (or emitting) a value into a new list.

What we need to do is of course format each value. But how?
We ask the compiler to figure that out for us.

So we say: go find us a formatter for T, the things in the List.
Then, we can use that formatter to format each value.

Now what if the compiler cannot find a formatter?
That’s a compiler error, which is good… it’s telling us we’ve not given the compiler the building blocks it needs.

With that inplace we can now output lists of values.

## How does that work? ##

We’ve said output this list of values.
Remember that outputJson lets the compiler go figure out a formatter for the value. The value here is a list of integers.

Can the compiler find a formatter for a list? Yes that’s this listFormat method we’ve just defined. To be able to do that, it needs a formatter for what’s in the list. Integers. Can it do that? Yes. Because we gave it an integer building block earlier.
So this works, and will output a list of integers.
Of course it will also work with a list of strings.

It may be a surprise, but it also works with lists of lists.
Because the compile says, can I find a formatter for a list? Yes, because of list format.  To do that it needs a formatter for what’s in the list. What’s that? Another list. Is there a format for that? Yes, it’s the same list format here.  
The compiler can recursively call implicit methods as needed.

## That is a type class ##

So this is incredibly powerful.
We refer to this pattern to this as a type class.

We have written small functions,
and combined them to create much larger capabilities.
This is used all over the place.

You want to parse or format Jon? Typeclass.
Read or write data in a database? Typeclass.
Convert from one format to another? Typeclass.

## Doobie ##

Our database uses typeclasses too.
We’ve not seen the database much.

The library I’m using here is called doobie.
You give it a SQL string, and it is able map columns into instances of classes.

Name and phone are probably stored in the database as VARCHARs.
ID is probably a serial primary key of some kind.

We need the basics building blocks to be able to read VARCHAR into a string, and a PK into a Long. 
And those are supplied by the library as they are so common.

But some something custom, like a subscription, we need to say how it’d be mapped to the database. And for that there’s a type class. You’d say implicitly if you see a subscription, this is how you convert it to and from a column.

Typeclasses are a veristive functional programming technique..


## Lunch ##

We're a good chunk of time into our day.
Over lunch we'll say we've encoded a problem with an ADT and we've used a type class for convert it to Json.
And you know what those things mean.

BTW, the libraries I’m using, weren’t designed to be used together.
The fact they use common patterns like ADTs and type classes mean that they work together pretty well.  And the same can be true of your code and libraries.

----

# 3. Putting it all Together #

## One more ticket ##

Let's pick up one last ticket before we call it a day.

So this is all about updating a customer record.
Once upon a time, this would be a form, you'd get all the details, and you'd write the changes to the database.
We don't tend to do that anymore.
Instead, we like rich, interactive pages, where when I make a change, that change is saved.

## Web layer ##

Let’s talk a look at the web layer of our application.
This is all we have at the moment: two end points.

This is a web server called http4s, and it lets us write patterns for web requests.
So you can see we have a GET request for /customers,
And we return 200 and a list of customers from the database.
There’s a type class that turns those records into JSON.

And we can also get a specific customer.
Looking up a customer returns an option. Maybe there’s a customer or perhaps there isn’t.  So we match on the two cases and return the appropriate response.

## Simplified patch ##

Let’s look at what kind of patch we might get.
Maybe we’ll just get an update to our name
Or perhaps will get a name and a phone number.
Or just a phone number.
We don’t know what it’ll be exactly.

But whatever we get, we want to merge it with an existing record.
We want to do this safely and flexibly.
We don’t want to use run-time reflection which might fail us.
We don’t want to generate code.
We don’t want to hand code all the permutations of what can be updated.
We want to know that if the code compiles, it’ll work.

We’ll receive a change and we need to update the customer.
From a functional programming point of view, it’s a functional from a customer and a patch to a customer record.

## Representing a patch ##

We can represent a customer and a patch pretty easily.
Well, you’ve seen customer already.
A patch is the fields we care about, but they are all optional.

How do we merge them together?
We’re going to use similar patterns to the ones we have seen.

## Pretend we can merge ##

We’re going to say: let’s imagine there’s a way to merge things,

Here’s our interface, our abstract representation of merging.
We have two types we care about.
The thing we’re merging into.
And the thing we’re merging from… the patch
And the result will be of the same type we started with.

Next we need to fill in the question marks with some real code.
And guess what… we’re going to make it the compilers problem.

Merging is easy, if the compiler can find a merge case class for the types we care about!
That’s the abstraction that allows us to now write the small pieces that can build up from.

## Merging for real ##

We could write this customerMerge by hand. We could take each field in turn and match them up. That’s OK.

But there are mechanisms for taking a class like customer and customer patch apart.  We’re not going into them today in the time we have. The basic idea is you take each field in the customer and patch, and ask the compiler to find a merge for them. A merge for String and Option of String. And that’s a small building block that we can easily write.

With those building block in place, we can merge larger structures from small pieces. So here we’ve patched the name of the customer.

## Putting it all together ##


Putting that back into code, we can write something that I think is reasonably easy to read and understand and maintain.

We’re decoding the request JSON (that’s a type class).
If that works, we know it’s worth bothering going to the database.
If the database finds a record, we can merge the patch.

I’m using a library for the merging here, from our colleague at Underscore, Dave Gurnell.  It follows the same principles we’ve seen, but is a bit more fancy. It can match up the names of the fields as well as their types. 

What we’ve ended up with here is the job done.
No run-time reflection.
If the program compiles you will merge the values.



----

# Summary #

## There's more ##

That’s probably about enough coding for one day. We’ve got some pretty exciting code going on here.

We’ve used ADTs, which are safe and versatile.
And we’ve described type classes, as a way of writing little bits of code and combining them into bigger behaviours.

There’s more to functional programming than those two things.
But these in particular are two powerful, practical tools that get used every day.
And anyone can use them. Maybe they are unfamiliar, but a bit of practice, it becomes natural.


## Scala ##

We use Scala as our programming language.
There are good reasons for that.

It has a modern powerful type system,
which supports functional programming.

And it runs on the JVM.
And there’s work active on compiling to native platforms as well.
It also compile to JavasSript to run on node and in browsers.

And those two things are important because you want to be able to use these tools and techniques anywhere, and sometimes you want to share implementation across JVM and Javascript, for example.

And that’s another point that helps make change easier.
And I hope that’s something you’ll take away from this talk.

Thank you.

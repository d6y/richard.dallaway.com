---
layout: post
comments: true
permalink: /:title
title: 'Annotations'
author: Richard
date: 2006/11/03
alias: /annotations-5
tags:
---

Of all the new language features in Java SE 5, [annotations][] were
probably the one thing I was nervous about. The benefits are clear for
things like the [Java Persistence API][], and there's no going back once
you've used them. For some of the other annotations, I didn't really see
the point. I do now.

I was chatting with Paul the other day about a feature of the language
neither of us fully [grokked][]. Here's the trimmed down example, and
it's perfectly legal Java that compiles and runs just fine...

	public class Cat
	{
	 // Cats can walk:
	 public void walk()
	 {
	   System.out.println("Walking with this many legs: "+getLegs());
	 }
	
	 // The walk() method needs to know how many legs to use:
	 private int getLegs()
	 {
	   return 4;
	 }
	
	}
	
	public class InjuredCat extends Cat
	{
	 // Just like a regular Cat, but three legs:
	 private int getLegs()
	 {
	   return 3;
	 }
	
	public static void main(String[] args)
	{
	   // Let's see how Tiddles walks...
	   Cat tiddles = new InjuredCat();
	   tiddles.walk(); 
	 }
	}


...but it didn't do what I expected it to, and I'm mentioning it because
I think it's an easy mistake to make. The code suggests that Tiddles
made a full recovery when it prints out
`Walking with this many legs: 4`. That could be a suprise, if you think
the `getLegs` method associated with the `InjuredCat` class to should be
called when we say `tiddles.walk()`; in fact it's the method in `Cat`
that's being called. That's the right result, and sometimes you want
code to do that, but in this case it was not the *intention*. And that's
the key word here.

Add in an annotation...


	 // Just like a regular Cat, but three legs:
	 @Override
	 private int getLegs()
	 {
	   return 3;
	 }


...to make the intent clear, and when now the code no longer
compiles—which is a good thing because it tells me the code won't do
what I expect it to do. To fix the problem, and make sure Tiddles has
three legs, I'd have to change `getLegs` to be `public`, or at least not
private, and then the override will have an effect.

[`@Override`][] is capturing the intent of the code: "Indicates that a
method declaration is intended to override a method declaration in a
superclass. If a method is annotated with this annotation type but does
not override a superclass method, compilers are required to generate an
error message." This approach is being extended in [JSR 305: Annotations
for Software Defect Detection][].

Apologies for hyping this, but it's another example of how the new
language features are helping developers to write better code, faster.

Regarding other Java SE 5 language features, [the generics tutorial
PDF][] is a great guide to understanding some of the trickier parts of
generics.

*No cats were hurt in the making of this blog.*

  [annotations]: http://java.sun.com/docs/books/tutorial/java/javaOO/annotations.html
  [Java Persistence API]: http://java.sun.com/developer/technicalArticles/J2EE/jpa/
  [grokked]: http://www.urbandictionary.com/define.php?term=Grokked
  [`@Override`]: http://java.sun.com/j2se/1.5.0/docs/api/java/lang/Override.html
  [JSR 305: Annotations for Software Defect Detection]: http://www.jcp.org/en/jsr/detail?id=305
  [the generics tutorial PDF]: http://java.sun.com/j2se/1.5/pdf/generics-tutorial.pdf


---
layout: post
comments: true
permalink: /:title.html
title: 'A comment on static typing'
author: Richard
date: 2007/08/17
alias: /a-comment-on-static-typing
tags:
---

There's a line of thought [put forward by Neal Ford][] to help Java developers get over any hang-ups they may have with dynamic typing. If you like compile-type checking of your types, maybe you're worried "that utter chaos would reign if we discard typing". Neil's argument is that: (a) unit testing is good; (b) 100% code coverage is good; therefore static typing buys you nothing:

> "If you have pervasive testing, *static typing == more typing*. The
> static typing is nothing but a requirement to type extraneous code to
> satisfy a compiler that isn't telling you anything interesting
> anymore."

It's a compelling argument ("[a]fter all, it's the tests that tell you
if your code [is] correct or not"), but I think it hides more that it
reveals. My main concern is that the argument assumes there's the same
amount of testing in both the dynamic and static cases. That doesn't
seem to be true: if the compiler is catching cases that you need to test
for in a dynamic language, then that's more unit tests that need to be
written. 

Let me phrase this another way:

> If you have static typing, *dynamic typing == more testing*. The
> dynamic typing is a requirement to type extraneous code to satisfy a
> code coverage tool that would otherwise have been caught by the
> compiler.

That's not to knock dynamic languages (I know first-hand how damn
productive Perl is): I'll use dynamic languages some times, and static
languages other times and admire them for what each can do for me in
different situations. And it's not to support the idea that 100% code
coverage is an achievable goal in practice. It's just that Neal Ford's
in principle argument doesn't work for me.

  [put forward by Neal Ford]: http://memeagora.blogspot.com/2007/05/strong-typing-is-communist-bureaucracy.html

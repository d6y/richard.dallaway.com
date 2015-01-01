---
layout: post
comments: true
permalink: /:title
title: 'JSESSIONID and the like, plus the Privacy and Electronic Communications'
author: Richard
date: 2011/07/22
alias: /jsessionid-and-the-like-plus-the-privacy-and
tags:
---

You know what I'm about to say, right?  I am not a lawyer.

But I do have to keep track of changes in the law that impact us and our
clients, and one of those is the Privacy and Electronic Communications
Regulations.  It's the one about getting consent to set cookies before
you set them.  There's a [nice short summary][] by Simon Weinberg.

We regularly use three groups of cookies in our applications:
JSESSIONID, ext\_id ([Lift][]'s "keep me logged in" functionality), and
the handful used by [Google Analytics][].  My impression is that we can
reasonably argue that JSESSIONID is an essential cookie for the use of
any of our sites.  The others are going to need consent, and at the very
least an update to all of our terms of service and privacy policy
documents.

All of the above are assumptions I'm making.  If you want an example of
how others are tackling this problem, take a look at the [Information Commissioner's Office][] site.

**The long answer**

Specifically regarding the ext\_id under one usage scenario, I did ask
what the guidance is:

> Hello
>
> I'm trying to understand if "keep me logged in" functionality falls
> under the new rules.  By that I mean the use of a cookie to enable a
> registered user of a site to visit the same site (potentially over
> many weeks or months) without the need to re-log in to gain access to
> their account.  This is not used for tracking or marketing. Currently
> we do not ask the user if they want to set this cookie: we are
> assuming the user is logged in until they explicitly log out.
>
> Do you have a view on this type of cookie use?

Here's the reply in case you're in a similar position, and I though the
repy was really well put together and useful:

> Thanks you for your recent enquiry regarding the new Privacy and
> Electronic Communications Regulations 2003.
>
> It is my understanding that no differentiation between cookies on the
> basis of their duration is made in the Privacy and Electronic
> Communications Regulations 2003 (PECR) as amended.  Therefore it is
> our guidance that cookies need to be consented to irrespective of
> their temporary nature, unless the requirements of the ‘strictly
> necessary’ exception are fulfilled.
>
> As there is no exception specifically for ‘login’ cookies, whether or
> not a login cookie is excepted from the basic consent rule will depend
> on whether that cookie falls within the ‘strictly necessary’
> exception.  There is not a straightforward or catch-all answer to
> whether a ‘login’ cookie will be ‘strictly necessary’. 
>
> What will be crucial in deciding whether a login cookie fits within
> the narrow exception of being ‘strictly necessary’ is whether the use
> of that login cookie is essential for a service requested by the user.
>  It is essential that both parts of the exception are met – that it is
> a service the user has requested, and that the cookie itself is
> strictly necessary to that service.  (Our guidance on this point, as
> you have no doubt already seen, is available on our website: Read the
> ICO’s advice to organisations about how to prepare for the new rules
> on cookies). 
>
> In some circumstances it may be that a login cookie falls within the
> ‘strictly necessary’ exception – in others it will not.  We have not
> provided more detailed examples regarding login cookies as it really
> will depend upon an individual website’s specific circumstances as to
> whether the requirements of the exception are met.  I can see a
> situation where it might potentially be argued that a login cookie is
> strictly necessary if a user has asked to login to a site to access
> certain information and a cookie is integral to that login or
> authentication process.  Alternatively, if the login is being imposed
> on the user for the purposes of the website operator, or if the use of
> a cookie is not strictly necessary to that login or authentication
> process then the requisite criteria of the narrow exception are
> extremely unlikely to be met.
>
> Whether consent will be needed in each case will depend upon whether
> the login cookie in question is strictly necessary for a service
> requested by the user.  Where the criteria of the exception are not
> met, and consent is needed, the nature of that consent will depend in
> part upon the function(s) which the cookie in question is to carry
> out.  As you will see from our guidance, the more privacy intrusive a
> cookie activity is, the more important it will be to obtain meaningful
> consent. 
>
> The definition of ‘consent’ is given in Directive 95/46/EC – the Data
> Protection Directive (which you can access online at:
> [http://eur-lex.europa.eu/LexUriServ/LexUriServ.do?uri=CELEX][]:31995L0046:en:HTML,
> consent is defined in article 2(h)) – as being:
>
> …any freely given specific and informed indication of his wishes…
>
> Whilst the fundamental definition of ‘consent’ will not change, as
> mentioned above, the level of information provided to obtain consent
> and the way in which consent is obtained may differ considerably
> depending on the privacy intrusion represented by the cookie activity
> in question.  There is therefore very definitely not one ‘catch all’
> solution to cookie consent. 
>
> You can find out more about consent on our website at:
> [www.ico.gov.uk/for\_organisations/data\_protection/the\_guide/conditions\_for\_processing.aspx][]
> (scroll down to the heading ‘consent’).
>
> As far as the legislation is drafted, there is nothing to stop one
> consent being obtained to the setting of all of those cookies
> simultaneously – that is, when the initial cookie is set/consented to -
> provided always that appropriate and informed consent is obtained in
> accordance with the requirements of the new rule.
>
> I hope this information is helpful.

Hell yes: I thought that was helpful and clear.

 

  [nice short summary]: http://www.mablaw.com/2011/07/cookie-law-ico-food-for-thought/
  [Lift]: http://liftweb.net/
  [Google Analytics]: http://www.google.co.uk/intl/en/analytics/privacyoverview.html
  [Information Commissioner's Office]: http://www.ico.gov.uk/Global/privacy_statement.aspx
  [http://eur-lex.europa.eu/LexUriServ/LexUriServ.do?uri=CELEX]: http://eur-lex.europa.eu/LexUriServ/LexUriServ.do?uri=CELEX
  [www.ico.gov.uk/for\_organisations/data\_protection/the\_guide/conditions\_for\_processing.aspx]:
    http://www.ico.gov.uk/for_organisations/data_protection/the_guide/conditions_for_processing.aspx

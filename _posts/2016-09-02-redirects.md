---
layout: post
title: "Redirects in Route53/S3"
author: "Richard Dallaway"
---

The trick for redirecting domains with AWS is to make sure the bucket name exactly matches the domain name.  These notes so that I don't forget this again.

<!-- break -->

# Problem

You have `oldsite.org` and `www.oldsite.org` that you want to redirect to `newsite.org` and `www.newsite.org`. You want to do this with AWS technologies only.

# Solution

Create buckets in S3 for the old sites, ensuring the bucket name exactly matches the domain name.
That is, create two buckets: `oldsite.org` and `www.oldsite.org`.

Set these buckets up however you want as static web sites.
For example, redirecting to another bucket such as `newsite.org`; or serving `index.html` content with [meta refresh headers](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta).

In Route53, create A records for each of `oldsite.org` and `www.oldsite.org`.
Select "Alias" as the type of record
The corresponding bucket name should appear in the "Alias Target" drop down for you to select.

The oldsite.org should now be serving newsite.org content.

# References

- [How to redirect domains using Amazon Web Services](http://www.holovaty.com/writing/aws-domain-redirection/), Adrian Holovaty, Oct 2013.

- [Hosting a Static Website on Amazon S3](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html), AWS Documentation, accessed 2 Oct 2016.


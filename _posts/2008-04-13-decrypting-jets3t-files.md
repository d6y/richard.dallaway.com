---
layout: post
comments: true
permalink: /:title
title: 'Decrypting JetS3t Files'
author: Richard
date: 2008/04/13
alias: /decrypting-jets3t-files
tags:
---

This post is going to be a bit niche. The scenario is that you've used
[JetS3t][] to backup data to [Amazon S3][] via the [synchronize][] tool,
and in particular you've used the `-c` option to encrypt the data. But
you've downloaded the file with another tool, such as [ForkLift][] or
[S3 Browser][]. How do you decrypt the downloaded file?

The default encryption is `PBEWithMD5AndDES`, and with that knowledge you
may be able to find a tool that can decrypt it for you. I went a
different way, and just hooked straight into the encryption utilities
inside JetS3t:

<script src="https://gist.github.com/3160130.js"> </script>

So with a bit of fiddling you can copy, paste and compile that (you'll
need the JetS3t library and supporting JARs, plus [Commons IO][]). Or
you can download [jets3t-decrypt-dist.zip][], which contains the source
and the reqired libraries. Once inside the ZIP you can run:


    java -jar Jets3tDecrypt.jar password encrypted.file > decrypted.file

The libraries and the `Jets3tDecrypt.jar` was packaged automatically by
[Netbeans 6.1][], which is a lovely touch from an IDE.


  [JetS3t]: http://jets3t.s3.amazonaws.com/index.html
  [Amazon S3]: http://www.amazon.com/gp/browse.html?node=16427261
  [synchronize]: http://jets3t.s3.amazonaws.com/applications/synchronize.html
  [ForkLift]: http://www.binarynights.com/
  [S3 Browser]: http://people.no-distance.net/ol/software/s3/
  [Commons IO]: http://commons.apache.org/io/
  [jets3t-decrypt-dist.zip]: http://download.spiralarm.com/blog/richard/2008/04/jets3t-decrypt-dist.zip
  [Netbeans 6.1]: http://www.netbeans.org/

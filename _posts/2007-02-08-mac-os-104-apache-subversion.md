---
layout: post
comments: true
permalink: /:title
title: 'Mac OS 10.4, Apache, Subversion'
author: Richard
date: 2007/02/08
alias: /mac-os-104-apache-subversion
tags:
---

I recently set up a Subversion server on my Mac OS 10.4 MacBook Pro. It
was, amongst other things, a test-run for installing SVN on an
[XServe][]. It was somewhat involved, so here's my reminder on how to do
this. Hopefully [when 10.5 ships][] a lot of this will just go away.

The goal is to: install subversion, convert an existing CVS repository
to SVN, set up Apache to access the repository, set up some access
restrictions, and get it running over HTTPS. The starting point is that
10.4 ships with Apache 1.3, and I'd already installed a SVN client from
a `.dmg`, but you need Apache 2 and to install Subversion from source to
get the Apache SVN module compiled up, so that really doesn't matter
(the Mac OS 10.4 server, BTW, ships with an Apache 2.0 instance for
"[evaluation purposes][]", but I'm going to ignore that as we want the
latest and greatest Apache really).

My plan: install Apache in to `~/Applications/apache/2.2.4/` and SVN
into `~/Applications/svn/1.4.3/` and to convert the CVS repository into
a SVN one in `~/Developer/svn/BIG_PROJECT` (obviously, adjust to your
own tastes).

Here we go...

**Step 0:** You need the Apple Developer tools installed.

The next five steps are all concerned with `cvs2svn`.

**Step 1:** Install [DarwinPorts][] (or is it [MacPorts][] now?). Ports
provides a way to download, compile and install open source projects
into `/opt`. The [DarwinPorts instructions][] are pretty clear: grab the
Tiger `.dmg` installer and do the `sudo port selfupdate` thing. One
gotcha here: the installer adds `/opt/local/bin` to your `$PATH` by
adding a line to `.profile.` If you've got a `.bash_profile` already,
you may find the `.profile` file being ignored, so adjust however you
like so that `/opt/local/bin:/opt/local/sbin` ends up at the start of
your `$PATH.` An `echo $PATH` will tell you if you're good or not after
install and starting a new shell.

**Step 2:** Install Berkeley DB, which you need for cvs2svn later. If
you want you can skip this step and discover later if you need it, but I
needed it. The command to perform this install is:
`sudo port install db42`. Two confessions at this point. DarwinPorts
understands dependencies and will go and get any additional packages
required by db42, but I don't know how many that will be for you because
I'd already used Ports to install the latest version of Ruby which
downloaded lots of packages. The second confession is that at the time I
ran the above command, Ports wasn't responding to my proxy
configurations and it couldn't download the package from the shell. To
get around this I manually downloaded the package (`db-4.2.52.tar.gz`)
in my browser and put it in `/opt/local/var/db/dports/distfiles/db4/`.
If you have problems like this, running the port command with `-v` can
help.

**Step 3**: Add Berkley DB support to Python, which is something
required by cvs2svn as [described by plasticfuture][]. The steps here
are to grab the [Python Berkeley DB bindings source][]
(`bsddb3-4.5.0.tar.gz` for me), extract it and run: 

	$ python setup.py build --berkeley-db-libdir=/opt/local/lib/db42\
	 --berkeley-db-incdir=/opt/local/include/db42
	
	$ python setup.py install --berkeley-db-libdir=/opt/local/lib/db42\
	 --berkeley-db-incdir=/opt/local/include/db42


**Step 4:** You're now set to [download and set up cvs2svn][]. I had a
tar.gz of an existing CVS repository, which I unpacked to
`~/Developer/oldcvs/BIG_PROJECT`. You need to create a `cvs2svn.options`
file to set up the conversion. I was going to create a new SVN
repository, and you can [download the cvs2svn.options][] I used (more or
less... search for `[CHANGE ME]` in that file and fix it up for your own
usage).

**Step 5**: Run the conversion with:
`cvs2svn-1.5.1/cvs2svn --options PATH_TO/cvs2svn.options`. If you have a
"big project" this may take a while, so while it's running you might
want to type the following to be told when it's done:
`say Conversion Complete`. I had a handful of errors about files
existing in the repository and also in the Attic. I `rm`-ed the Attic
versions of the files and re-ran the command.

Now that we have a converted CVS repository as a SVN repository we can
continue and set up the server. Before that, I just did a sanity check
with `svn co file:///Users/richard/Developer/svn/BIG_PROJECT` into /tmp
or wherever to ensure there were no problems with the conversion
(remember: I already had the SVN client installed, but if you don't
you'll be able to do a test checkout around step 7, below).

**Step 6:** Install Apache. There's a pretty nice [Apple Developer
article on Subversion and Apache][] which is worth a read. All I did in
the end was to [download Apache][], unpack it, and run:\


	$ ./configure --prefix=/Users/richard/Applications/apache/2.2.4 \
	 --enable-mods-shared=most --enable-ssl --with-mpm=worker\
	 --without-berkeley-db --enable-proxy\

The `--without-berkeley-db` is ironic after the BDB hoops we've jumped
through above. The usual `make` and `sudo make install` and you're done.
Start apache with `sudo ./apachectrl start` from the
`~/apache/2.2.4/bin` directory and check that [http://127.0.0.1/][] is
running and then stop Apache. (Warning: if you just run `apachectrl`
you'll pick up the Apple-installed Apache 1.3 instance, so remember to
use the `./` or use the full path to the ` apachectrl` for the Apache
you've installed).

I ran into a few segmentation faults (or bus errors, depending on which
user you run as) when starting up Apache. It turns out there are a
couple of modules that fail to load or other issues in the configuration
file. One useful way to see how far Apache is getting is to validate the
configuration with debugging on: `sudo bin/httpd -t -e DEBUG`.

We'll configure Apache later, but first...

**Step 7:** Install subversion. Again, the Apple Developer document
above is helpful here. I downloaded the subversion source from
[subversion.tigris.org][] and ran...\


	$ ./configure --prefix=/Users/richard/Applications/svn/1.4.3 --with-ssl
	 --with-apxs=/Users/richard/Applications/apache/2.2.4/bin/apxs \
	 --with-zlib --enable-swig-bindings=no --without-berkeley-db \
	 --with-apr=/Users/richard/Applications/apache/2.2.4/ \
	 --with-apr-util=/Users/richard/Applications/apache/2.2.4/\

...followed by `make` and `sudo make install`. If you don't already have
a Subversion client on your `$PATH,` you can add
`~/Appliations/1.4.3/bin` now.

If you're going to go on to install Continuum or anything else that
might require http or https support from the Subversion client, you'll
want to also download the Neon library as documented by [The Punk
Engineer][] and indeed also by the Subverison `INSTALL` text file.

**Step 8**: Configure Apache. Now comes the Apache voodoo to turn on SVN
access. Edit `~/Applications/apache/2.2.4/conf/httpd.conf`. (BTW,
[TextMate][] is quite nice for all of this, partly because it recognizes
and colours Apache configuration, but also because from the shell you
can run `mate .` to open all files in a nice folder view.) At the bottom
of the Apache config add the following:

	<IfModule dav\_svn\_module>
	<Location /repos>
	DAV svn
	SVNPath /Users/richard/Developer/svn/BIG_PROJECT
	</Location>
	</IfModule>


Start Apache and in your browser you can hit [http://127.0.0.1/repos][]
to browse the source.

**Step 10**: Add authentication. Go back to `httpd.conf` and add in some
form of authentication and access control. Here's what I did...

	<IfModule dav_svn_module>
	<Location /repos>
	DAV svn
	SVNPath /Users/richard/Developer/svn/BIG_PROJECT
	AuthzSVNAccessFile conf/svn-access.conf
	
	Require valid-user
	AuthType Basic
	AuthName "Subversion Access"
	AuthUserFile /Users/richard/Developer/svn/svn-pwd-file
	</Location>
	</IfModule>


There are two changes here. The first is a `svn-access.conf` file that
controls what users or groups can do to parts of the repository. This
file looks like this:

	[groups]
	dev = paul, gareth, richard, andrew, jono
	
	[/]
	@dev = rw


It's saying there's a group of users call "dev" to have read+write
access to the "/" path in the repository (i.e., the lot). I wasn't able
to find a great deal of documentation on this, but thank you to Richard
Tesh for spotting the [archlinux.org][] guide.

The second part of the Apache configuration is the basic authentication
part, which is straight-forward non-weird Apache stuff. The command you
need is `htpasswd`: run it with `-c` the first time only to create the
`svn-pwd-file`. Use this to create a username and password for each of
your "dev" users.

Quit your browser, restart Apache, and try to hit the repository again:
This time you'll be prompted to login.

**Step 11:** Set up permissions on SVN repository. A gotcha here is that
the Apache user (daemon, group daemon) needs write access to the
repository, so you need to decide what you're going to do there. One
option is to `chmod` the repository, which is what I did.

**Step 12:** Finally you'll want all of this to run over HTTPS, and so
you'll need to set up a certificate for Apache. I'm using a self-signed
certificate. There's some great documentation for this, including the
ever-useful [macosxhints.com][]. I created a folder in my home directory
called `keys`, and from there I ran the following:


	$ /System/Library/OpenSSL/misc/CA.pl -newca
	$ openssl genrsa -des3 -out webserver.key 1024
	$ openssl rsa -in webserver.key -out webserver.nopass.key
	$ openssl req -config /System/Library/OpenSSL/openssl.cnf -new \
	   -key webserver.key -out newreq.pem -days 3650
	$ /System/Library/OpenSSL/misc/CA.pl -signreq


You'll get prompted to enter a bunch of information. Use your hostname
as the common name.

When those commands are done, you'll have a set of files and a
subdirectory in `~/keys`. Now to configure Apache, by adding the
following the `httpd.conf`:

	<IfModule mod_ssl.c>
	Listen 80
	Listen 443
	
	<VirtualHost _default:443>
	SSLEngine on
	ServerName 127.0.0.1
	SSLRequireSSL
	
	SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM: \
	 +LOW:+SSLv2:+EXP:+eNULL
	SSLCertificateFile /Users/richard/keys/newcert.pem
	SSLCertificateKeyFile /Users/richard/keys/webserver.nopass.key
	SSLCACertificateFile /Users/richard/keys/demoCA/cacert.pem
	SSLCARevocationPath /Users/richard/keys/demoCA/crl
	</VirtualHost>
	
	</IfModule>


Restart Apache and you can now hit `https://127.0.0.1/repos/BIG_PROJECT`
or enter that into whatever tool you use to access subversion, such as
an [Eclipse plugin][]. You'll be told the certificate isn't recognized,
but just "continue" or "OK" or "accept" your way past that. BTW, if you
try to hit the non-HTTPS URL, you'll be forbidden.

Aaaaaaaaand we're done. Apart from backups, more sensible security,
better file naming, multiple repositories, automated builds....

Thank you: [Paul][], [Pragmatic Version Control Using Subversion][] and
all the HOW TOs I've not mentioned.


  [XServe]: http://www.apple.com/xserve/
  [when 10.5 ships]: http://upcoming.org/event/149426/
  [evaluation purposes]: http://docs.info.apple.com/article.html?path=ServerAdmin/10.4/en/c5ws8.html
  [DarwinPorts]: http://darwinports.opendarwin.org/
  [MacPorts]: http://www.macports.org/
  [DarwinPorts instructions]: http://darwinports.opendarwin.org/getdp/
  [described by plasticfuture]: http://blog.plasticsfuture.org/2006/03/04/install-cvs2svn-on-mac-os-x-1042/
  [Python Berkeley DB bindings source]: http://pybsddb.sourceforge.net/
  [download and set up cvs2svn]: http://cvs2svn.tigris.org/cvs2svn.html
  [download the cvs2svn.options]: http://blog.spiralarm.com/richard/2007/02/cvs2svn.options
  [Apple Developer article on Subversion and Apache]: http://developer.apple.com/tools/subversionxcode.html
  [download Apache]: http://httpd.apache.org/
  [http://127.0.0.1/]: http://127.0.0.1/
  [subversion.tigris.org]: http://subversion.tigris.org/
  [The Punk Engineer]: http://www.latke.net/blog/punkengineer/archive/000046.html
  [TextMate]: http://macromates.com/
  [http://127.0.0.1/repos]: http://127.0.0.1/repos
  [archlinux.org]: http://wiki.archlinux.org/index.php/Subversion_Setup#Apache_Installation
  [macosxhints.com]: http://www.macosxhints.com/article.php?story=20041129143420344
  [Eclipse plugin]: http://subclipse.tigris.org/
  [Paul]: http://www.goulbourn.com/
  [Pragmatic Version Control Using Subversion]: http://www.amazon.co.uk/dp/0977616657?tag=richarddallaway&camp=1406&creative=6394&linkCode=as1&creativeASIN=0977616657&adid=0GP0G2BKXGZSB9F4V27J&


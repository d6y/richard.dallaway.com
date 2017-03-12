---
layout: post
comments: true
permalink: /:title.html
title: 'OpenStreetMap'
author: Richard
date: 2007/01/21
alias: /openstreetmap-38
tags:
---

[Mikel Maron][]'s fireside-style talk at the [Sussex Geek Dinner][] was
a great introduction to [OpenStreetMap][]: "providing free geographic
data such as street maps to anyone who wants them". If you need to know
anything about this area, Mikel is clearly the person to talk to.

[Jane][]'s blogged about the talk, so I'll skip that and just mention
that the talk inspired me to take another look at the Bluetooth GPS unit
we have. Turns out it's easy to get the GPS unit running with a MacBook
[via the terminal][]: pair with the device, and then in Bluetooth
Preferences, select the device and check the serial ports. In my case, I
didn't have to do anything, as the Mac had already set it up as a serial
device. From the shell, you can then connect to the device...


    $ screen /dev/tty.BT-GPS-3404F4-BT-GPSCOM-1

...and it starts spewing data:


	$GPGSA,A,3,20,01,11,17,,,,,,,,,2.6,2.4,1.0*34
	$GPRMC,164654.000,A,5049.4955,N,00008.8944,W,0.06,87.03,180107,,*2C
	$GPGGA,164655.000,5049.4957,N,00008.8945,W,1,04,2.4,26.0,M,47.0,M,,0000*71
	$GPGSA,A,3,20,01,11,17,,,,,,,,,2.6,2.4,1.0*34
	$GPRMC,164655.000,A,5049.4957,N,00008.8945,W,0.09,24.77,180107,,*2B
	$GPGGA,164656.000,5049.4959,N,00008.8946,W,1,04,2.4,26.3,M,47.0,M,,0000*7C
	$GPGSA,A,3,20,01,11,17,,,,,,,,,2.6,2.4,1.0*34

These sentences aren't too scary, once you have the [NMEA][]
descriptions. And as it's Bluetooth and a serial connection, you can
imagine it's [not too bad][] to get that data live on a phone, without
the need for [JSR-179][] or [JSR-293][]. Great fun.

But back to the talk, I should mention that the big thing OpenStreetMap
has is a great refresh rate. Compared to other mapping organizations,
OpenStreetMap contributors can, for example, spot new streets in their
area as they are being built, meaning the OpenStreetMaps maps can be
years ahead of the other players. On the other hand, organizations like
the Ordnance Survey have a great advantage in precision. But what you
need depends on what you want to do, and for OpenStreetMap the focus (I
think) is about navigation.

Links to follow up: [New Popular Edition][] (OS maps out of copyright);
[Neogeography][]; [Free The Postcode][]; [KML][]; [Geonames][]; [OS information][]; [GPSBabel][]; [Yahoo! allows satellite imagery in OpenStreetMaps][].

  [Mikel Maron]: http://brainoff.com/weblog/
  [Sussex Geek Dinner]: http://sussex.geekdinner.co.uk/
  [OpenStreetMap]: http://www.openstreetmap.org/
  [Jane]: http://jane.dallaway.com/blog/2007/01/sussex-geek-dinner.html
  [via the terminal]: http://discussion.forum.nokia.com/forum/archive/index.php/t-54630.html
  [NMEA]: http://aprs.gids.nl/nmea/
  [not too bad]: http://www.google.com/search?q=java+nmea+parser
  [JSR-179]: http://jcp.org/en/jsr/detail?id=179
  [JSR-293]: http://jcp.org/en/jsr/detail?id=293
  [New Popular Edition]: http://www.npemap.org.uk/
  [Neogeography]: http://www.oreilly.com/catalog/neogeography/
  [Free The Postcode]: http://www.freethepostcode.org/
  [KML]: http://earth.google.com/kml/
  [Geonames]: http://www.geonames.org/
  [OS information]: http://www.ordnancesurvey.co.uk/oswebsite/gps/information/coordinatesystemsinfo/guidecontents/index.html
  [GPSBabel]: http://www.gpsbabel.org/
  [Yahoo! allows satellite imagery in OpenStreetMaps]: http://www.opengeodata.org/?p=120


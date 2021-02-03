---
title: "Aptana Studio and GCJ"
date: 2015-10-27 21:09:23
published: true
categories: [development]
wpid: 496
---

Aptana Studio is a pretty darned impressive IDE for web-related development. I use it almost exclusively. The installation instructions on the download page make reference to 64bit users having to utilise the eclipse plugin version rather than standalone. The problems occur when you install Eclipse from the Ubuntu apt repositories.

When you install Eclipse, via apt, the package "eclipse-gcj" gets installed which also pulls in the gcj runtime. This seems innocuous in itself, but the eclipse startup script checks for existence of various java runtimes to determine which it will use. The problem is the list is checked from top to bottom with the first matching runtime being utilised. This means that even when you have a more capable and complete java runtime (Sun Java 6), eclipse still uses gcj. This is a major problem, as various dialogues don't work/appear and lots of random things are broken.

When you're using gcj, and you install Aptana, the installation proceeds nicely and you get to the restart SDK phase. Once Eclipse reloads, you are alerted with various runtime errors and a nice dialogue from Aptana itself reporting that Aptana doesn't support running on any runtime other than Sun's, and that you are currently running on version 1.5 from the Free Software Foundation.

The solution to this problem can be one of two methods. The first is to manually alter the search paths to place sun higher than gcj. This involves editing `/etc/eclipse/java_home`. The other method requires creating a file in your eclipse user configuration directory. The contents of which are as follows:

```bash
#!/bin/sh
JAVA_HOME=/usr/lib/jvm/java-6-sun
export JAVA_HOME
```

This file should be saved to `~/.eclipse/eclipserc`, and should have the correct path to your sun java directory. (On ubuntu all java runtimes are installed to `/usr/lib/jvm`.)
---
title: "Hackintosh"
date: 2007-08-10 18:47:03
published: true
categories: [random]
wpid: 96
---

[UPDATE: see my new post here](/osx86-snow-leopard-hackintosh/)
published: true

Wowzers!

After I had my mac mini arrive just two weeks ago, I've been impressed no-end by the OSX operating system. So much so that, now that I'm sending my mac mini back for an upgrade under the 14-day remorse period (they upgraded the mac mini line on Tuesday to a much more advanced unit), and I'm to be without my beloved OSX, I decided to have another go at getting it installed on my beige box PC.

So, here's the specs on the machine:

> MSI 975X Platinum Power-up Edition Mobo, 2GB RAM, Core 2 Duo E6600 2.4GHz, 250GB HDD, DVD Burner, and nVidia graphics.

The dvd wouldn't boot directly, and needed to be tickled with a little jiggery-pokery. First I needed to move the dvd drive onto the primary IDE channel with the HDD so that the BIOS would recognise that it existed without using the JMicron second channel which is unsupported by OSX at this time. Second I needed to boot the DVD with the -v option to get past the "/com.apple.boot.plist not found" error (I think that was the error anyway).

Now that I was into the dvd interface, I ran the disk utility to erase my hard drive. This was a dead cinch. Onto the install phase, I selected the optional titan nVidia drivers for my GeForce 7600GS. Once I was in the installed operating system after the reboot I downloaded ~~ALC888Audio.mpkg from the InsanelyMac forums~~ and ran that. After another reboot I then found that I had fully operational audio and graphics with Quartz Extreme and Core Image support.

EDIT: Now, don't assume that I'm using this machine in favour of a real mac. Quite the contrary, I intend on getting either a Mac Mini (to replace the one I'm sending back – see above re: remorse period), or a more expensive MacBook, or an even more expensive iMAC which would replace this desktop unit completely. So, either way, I will be buying a full-on pukka bona-fide Mac.

EDIT2: I bought a 24Inch iMac.
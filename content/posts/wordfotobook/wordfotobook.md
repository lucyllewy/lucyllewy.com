---
title: "WordFotobook"
date: 2015-10-27 21:09:23
published: true
categories: [random, WordPress]
wpid: 246
---

This project is no longer actively developed. Older relases are still linked to for archival purposes.

I've noticed in the latest release of wordbook, that the author has put in place a simple check to determine whether the facebook "class" has already been loaded. This is all very well and good, but there is no sure way to determine that wordbook will be the last plugin loaded by WordPress that utilises the facebook api. So, while the combined package of wordfotobook is deprecated, I still need to release an updated version of the fotobook component, which has the same check put in place. To this end, I will no longer be releasing updates on this page, but will instead be focussing on keeping the simple check enabled in my '\[\[Fotobook+Thickbox Integration (and wordbook compatibility)\]\]' page.

~~[WordFotobook 0.3](/files/2008/11/wordfotobook-03.zip)~~

And now for a third release, but this one is only minor. I've modified my fotobook thickbox integration and unified it with my patch which I have submitted upstream. At this moment in time (23/Nov/2008), this is the \_only\_ place that I know of where you can get fotobook with thickbox support, and the ability to run wordbook and fotobook together.

~~[WordFotobook 0.2](/files/2008/11/wordfotobook-02.zip)~~

Second release, made just hours after the first public release of WordFotobook. This release upgrades fotobook to 3.1.5, and fixes my constant misspelling of Fotobook as "Photobook". New in this release is support for the thickbox method of displaying the photos in addition to lightbox; the 0.1 version had partial support. Changes will be submitted upstream tomorrow (23/11/2008 – that is, of course, if I remember)

~~[WordFotobook 0.1](/files/2008/11/wordphotobook-01.zip)~~

Initial release contains Wordbook 0.14.2 and Fotobook 3.1.3 capable of running together concurrently with no ill effects from double-importing the Facebook library.
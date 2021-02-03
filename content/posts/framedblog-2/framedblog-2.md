---
title: "FramedBlog"
date: 2015-10-27 21:09:23
published: true
categories: [random, WordPress]
wpid: 146
---

As a precursor to the return of a music player for my blog, like that of CuBeZeRo's site (~~www.nweightman.co.uk~~), I have inserted a frameset so that I can have the player permanently at the top of the window while the user is navigating the site. This allows me to use the streaming mp3 technology of Adobe Flash (my choice of player) rather than embedding the mp3 into the flash file making it a huge-ass download.

Using streaming also means I can have more than one mp3 file and either rotate them as they play, or change the single file once in a blue moon when I feel like a change, without having to recode the flash file.

Now onto the clever bits: I've coded a plugin for WordPress that simply adds some javascript to the top of each page served, and placed counter code in the frameset page. These codes combine to allow the user to deep-link their bookmarks to any page on the site while still retaining the frameset. I've utilised anchors ) for this, and the javascript inserts the correct page's url as the anchor whenever the user clicks a link.

When the frameset is loaded from a bookmark that has an anchor the javascript sets the content frame's url to the correct location. And finally, on each loaded page of the blog there is javascript that sets the browser's title to the correct string, rather than having a single static title for the entire site.
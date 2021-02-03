---
title: "htmlwrapper"
date: 2009-11-04 06:05:41
published: true
categories: [random, WordPress]
wpid: 576
---

I've been looking for ages for a way of getting WordPress to work with Adobe's flash technology, to create a more powerful and dynamic site. To this end I recently discovered [htmlwrapper](https://code.google.com/p/htmlwrapper/) from <span class="removed_link" title="https://motionandcolor.com">Motion &amp; Color</span>. This unique project takes xhtml and renders it using the flash runtime. The advantage of this is that the whole world of flash is then open to the site owner allowing much more dynamic experiences.

Interactivity is created using JSON-based function calls embedded in the site's CSS stylesheets. The big bonus is that the content of each page is still visible and browsable by the search engine spiders, while still keeping everything rendered by flash. There are even features to arbitrarily draw random "sprites" such as stars/ovals/etc. without having to create (possibly large) images of each with transparencies where you need them.

Now for my news: My blog will eventually be ported across to the flash rendering engine, but more than that I've been accepted as a member of the project's team. With some spare time on my hands this evening, I've been answering issues raised in the bug-tracker and fixing problems that I've discovered in the sourcecode.
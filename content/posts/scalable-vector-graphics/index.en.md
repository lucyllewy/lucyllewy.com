---
title: "Scalable Vector Graphics"
date: 2009-07-11 03:03:58
published: true
categories: [development]
tags: [svg, webdev]
wpid: 521
---

I've been fiddling with my site design some more recently, and decided that I wanted a vertical banner on the left-hand side to advertise ~~IND-Web.com's Free Blogs and Hosting~~ services. I figures that I'd only be able to manage this with an image, and set about creating said image in a drawing application. I then found that the image I had created didn't scale nicely nor did it fit on pages of arbitrary height without leaving a big gap of non-clickable background colour or, worse, distorting the image so that the text was illegible.

The solution I found is to use SVG. And, a big bonus for me, I found that images of this type are defined with XML text and not binary gibberish. As it's XML I can program my graphics instead of designing them. And, I can also add arbitrary javascript code that can alter the image on-the-fly just like xHTML only with graphical dohickies and effects such as arbitrary rotations.

Text in SVG is written very similar to xHTML, just with a different `<tag>` name (being "text"). A big advantage of this way of defining text is that I don't need to do anything special to change what the advert says as I can just open the .svg file in my favourite text editor and replace the content of the `<text>` tag.

In the future, the W3C have said that SVG should be able to be embedded directly within an xhtml document. This will mean that javascript will be able to alter both xHTML elements *and* SVG elements without any fancy wrappings.

So far I've only scraped the surface of what's available in the SVG format, but I'm eager to see what else I can do with it.
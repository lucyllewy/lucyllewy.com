---
title: "WP Super Cache oopsiedoodle"
date: 2010-10-26 21:26:17
published: true
categories: [random]
wpid: 736
---

Yeah, so why does WP-Super-Cache have to hard-code it's file paths into the PHP to work correctly? What I'm talking about here is -content/advanced-cache.php which is created by wp-super-cache when you activate it the first time. That would be all well and good, but it chooses to use absolute file-system paths based on your individual server's layout.

Why is that bad? Well consider this: IND-Web.com recently migrated all it's files to a different directory on the server to allow for better maintenance. Unfortunately this new directory was not the same path as the old directory and therefore the hard-coded path in advanced-cache.php was no longer correct, which meant that WP-Super-Cache was broken.

The real crux of the issue, though, is that there is absolutely NO mention of this in the wp-admin dashboard or even in wp-super-cache's setup screen. The only evidence of the problem was an HTML comment at the base of every page which informed of the situation. While you might think that's good, it's not, because of two reasons: 1) it informs hackers that they are able to more effectively DDoS your server and 2) HTML comments are invisible in the rendering of the page and are only seen when you "view source" in your Web Browser.

This lack of visibility of the problem meant that IND-Web.com, along with *URL-GONE* were both completely un-cached and susceptible to floods of traffic overloading the processing capability of the server through re-creating each page upon every visit; as opposed to creating once when each page is updated and then serving the static cached copy to subsequent requests instead.
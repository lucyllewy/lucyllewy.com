---
title: "Printing CSS Pages"
date: 2009-04-14 19:19:08
published: true
categories: [development]
tags: [printing, css, webdev]
wpid: 461
---

I thought I'd post a revelation I've had while designing my brother's site (as was briefly mentioned in my previous post). HTML and CSS based designs are actually more versatile than I originally thought. I've long been aware that you can use `media="print"` and `media="screen"` to separate out differing CSS rules for print media. However, I didn't know just how useful this can be..

Take links, for e.g: in print media a piece of blue underlined text is pretty useless, as you cannot determine where the link originally pointed, only that a link existed. However, with a small snippet of CSS wizardry, you can get the browser to insert the link's URL after the textual content of the link itself.

```css
a:link:after, a:visited:after {
    content: " (" attr(href) ")";
    font-size: 80%;
}
```

As you can see in the above snippet, I'm using the "content" attribute to specify that `(<link URL>)` be inserted after (`:after`) the initial content of the `<a>` tag. I am also specifying that the text be `80%` of the normal text size so that it is more obvious that the URL is not part of the original content.

The only problem with this, is that links will be printed verbatim, so any "rooted" URLs will just appear as `/internal/path/to/document.html` instead of `https://www.example.com/internal/pa....` This can be solved with more advanced CSS in supported browsers with the following rule set:

```css
a[href^="/"]:after {
    content: " (https://www.example.com" attr(href) ")";
    font-size: 80%;
}
```

This snippet takes all `<a>` tags that begin (`^=`) with `/` and insert `https://www.example.com` before the local URL to make it canonical.
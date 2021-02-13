---
title: "minor improvements"
date: 2009-03-24 09:48:17
published: true
categories: [random]
wpid: 341
---

I've made a few tweaks to my blog's theme. The changes include:

- adverts in the header alternate between google and amazon, and disappear off the top of the screen as the user scrolls down, so as not to interfere with reading.
- adverts to the right of the sidebar, again so as not to interfere with reading or navigation. Again, these are random adsense and amazon.
- sidebar made position: static so that navigation is always visible
- footer made position: static so that copyright is always visible
- sidebar panels shrunk so that smaller displays are supported
- sidebar width is now controlled differently – `left: 67%; right: 160px;` that way the right of the sidebar will never overlap the adverts (or the adverts overlap the sidebar) and the sidebar will always appear exactly butted-up-against the content pane.
- finally, the content pane has been split in two for über-recent browsers (safari 3+, firefox 3.1b3+)
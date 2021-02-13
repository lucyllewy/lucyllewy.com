---
title: "Links work again"
date: 2014-07-04 03:04:13
published: true
categories: [random]
wpid: 681
---

OOPS! With some minor tweaking to the [CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) that determines how my site looks when viewed in a [web browser](https://en.wikipedia.org/wiki/Web_browser) I managed to completely kill all clickable links. The good news is that I've since spotted the mistake and have implemented a fix. The issue was with the "view in mobile" text at the very bottom of every page. In effect my changes had made this item be the entire height of the document and overlaid on top of everything else. (Z-order or [Z-index](https://en.wikipedia.org/wiki/Z-index) – think of the [Z axis](https://en.wikipedia.org/wiki/Cartesian_coordinate_system) being depth coming out of your monitor towards your face, and items with a higher z-index are closer to your face and appear on top of those items with lower values.)

Essentially I had set the "view in mobile" text to be above all the rest of my content and the height of the content. The reason that this killed my links is that the web browser interpreted "click" events to be targeted at the "view in mobile" element instead of the link that the user wanted. Think of a sheet of paper coving a printed document; you can't do anything with the printout until you first move the blank sheet away. My fix was to specify a height attribute in my CSS, making my overlay much shorter than originally, and therefore it no longer covered anything important such as clickies.
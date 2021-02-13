---
title: "Chrome Frame and SVG"
date: 2009-09-30 19:21:15
published: true
categories: [development]
wpid: 546
---

This is a followup to [my article on SVG](/2009/07/svg).

I was contacted in the comments of my SVG article by Brad Neuberg who suggested that I utilise SVGWeb to display my SVG files in IE. Well, I downloaded and started using it, but it appears that there is a slight bug in the handling of text-anchor:end in the Flash-based renderer. When using SVG Fonts, I guess this is a non-issue, but I'm using built-in fonts, which aren't able to be rotated without a hack to the SVGWeb Source.

The reason for the lack of rotation is also the reason, I guess, that the text-anchor:end is also causing issues. Suffice it to say that I got the rotation working per the bug report on svgweb. However, the text is positioned about 325pixels, which I believe to be half the width of the text, off in the x axis (horizontally or, when rotated through 90 degrees, vertically; as the x axis is also rotated through the same 90 degrees).

So, instead of waiting for a fix to this issue, I have implemented Google's Chrome Frame on my site, and have put in place a piece of javascript which will force users to install said software if they are viewing in Internet Explorer. Chrome Frame is a hack to internet explorer that forces it to use the Google Chrome rendering engine instead of Internet Explorer's own. This means that I can use the more up-to-date features of Google Chrome without worrying that users of Internet Explorer will get a messed-up view of my site. All the alternative browsers are about level in terms of feature parity, however IE is lagging behind, and doesn't know anything about HTML5.

HTML5 is the latest and greatest version of the language in which webpages are written. It has many new features and "semantics" which allow the site designer to program their pages with more "meaning" rather than just relying on the perception induced by visuals. Semantic "Markup" allows readers with special requirements to understand the meaning behind the text without having to "see" visual differences. One example of this is to have a blind person's computer read the text in the correct order and with the correct meaning, such as having the headings and footers given much less prominence than the actual articles.

So, readers, if you prefer using IE, but still want to be able to view my site, I urge you to install Chrome Frame, and try my site again. Alternatively, install [Google Chrome](https://www.google.com/chrome/) or [Mozilla FireFox](https://www.getfirefox.com/) or [Apple Safari](https://www.apple.com/safari) instead and have a more up-to-date experience of the web for _every_ site instead of just those that opt-in to Google Chrome Frame. (although, you _can_ see what a site would look like inside Chrome with Chrome Frame installed by prefixing the address with "cf:" to force IE to use Google's rendering engine. e.g. [cf:https://bowlhat.net/](https://bowlhat.net/) – note that the previous link will only work for IE users with Chrome Frame installed.)
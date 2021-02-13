---
title: "Bendy Corners"
date: 2015-10-27 21:09:23
published: true
categories: [random]
wpid: 226
---

> Ooh, Ahh, Just a little bit.  
> Ooh, Aah, A little bit more!
> 
> <cite>Gina-G</cite>

So I wanted that little bit more eye-candy on my site. To fix this, I made a very minor tweak to my CSS (Cascading Style Sheet) code – CSS is what changes web page code into something visually appealing.

The change makes use of a very new edition of CSS called version 3, which not all web browsers support yet. Due to this, you may not see the "bendy corners" that I've implemented on the main page (see the top left of the welcome note: if it's square then you don't have a recent-enough web browser to see the effect). To use the effect in your own pages, the css code follows:

```css
border-radius: 25px 6px;
-moz-border-radius: 25px 6px;
-webkit-border-top-left-radius: 25px;
-webkit-border-top-right-radius: 6px;
-webkit-border-bottom-right-radius: 25px;
-webkit-border-bottom-left-radius: 6px;
```

The reason for the `-moz` and `-webkit` versions, is due to the fact that mozilla and apple (respectively) implemented their versions before the official standard was "ratified". \[turned into a rat, I'm assuming that means.\] Newer versions of both browsers will move over to the official border-radius symantic. Opera supports this feature using the propper code out-of-the-box, IE8 beta supports it, Firefox supports it in version 3 and above using the `-moz` prefix, Safari supports it using the `-webkit` prefix, and finally Konqueror does NOT support it yet.
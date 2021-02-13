---
title: "AJAX Blog"
date: 2008-07-10 01:06:47
published: true
categories: [random]
wpid: 206
---

The observant amongst you may have noticed a new page appear in the navigation, or that the bar down the right hand side of each page within this site has disappeared. Well, the sidebar is still there, just hidden. To get at all the functionality of the sidebar, one just needs to click the little arrow to the right of every page under the sunset (or the vertical text underneath that) and the sidebar will be revealed by some technological wizardry written by myself with the aid of the fabulous script.aculo.us+prototype.js programing libraries (though I would like to reimplement it using YAHOO's YUI, which is even more fabulous than scriptaculous!!).

Also, so tells the new page I mentioned earlier, page loading is all done in the background using what is known as AJAX. This acronym means "Asynchronous Javascript and XML".

The XML part I can explain straight away to mean my web pages, as the language that webpages are written in is a SuperSet of XML (meaning that you can pass a webpage through an xml program and still get a meaningful output).

Javascript is the language that adds interactivity to otherwise dull and lifeless static web pages. It is at the very heart of the so-called "Web 2.0" movement, and drives popular sites like google maps and gmail, or yahoo mail.

The asynchronous part is the interresting bit, meaning that you can do other things while the page is loading. For example, you could click a link on my site to another page within my site, and while it's loading you can still listen to the music in the top right-hand music player (to be fixed) without interruptions. Even once the page has loaded and the "Browser" program you are viewing my site through is displaying the new content, the music will carry right on playing, and the sidebar will be fully accessible throughout the loading process.

Now for tidbit stuff: Google has a service called "analytics" which provides website owners a thorough breakdown of the site's audience's actions while on the site that is being analised. This is done through yet more javascript which the site owner (in this case me) adds to each and every page on the site. (In reality that means just adding it to the "template" that the website uses to insert the content into so that each page has the same look about it).

However, when the pages are being loaded by AJAX, the javascript isn't reloaded, meaning that all google ever sees is the user spending a lot of time on one page with their thumb up their proverbial posterier. The code that I wrote for my AJAX Blog stuff does some clever behind-the-scenes calls to google's analytics code which forces it to acknowledge a new page has been visited when the user clicks a link that merely runs some javascript code instead of loading a brand new page.

This allows me to see where people are going on my site, and work out what type of content people like reading so that I can tailor future articles to match what my readers are reading (not that I have readers.. I know this is merely a backwater of the internet that is viewed only by my friends and family, but it keeps me happy, so nyer!).
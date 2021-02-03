---
title: "OAuth+OpenID"
date: 2015-10-27 21:09:23
published: true
categories: [random]
wpid: 686
---

I'm currently working on getting [OAuth](https://oauth.net) and [OpenID](https://openid.net) working on my blog so that I can use my [URL](https://en.wikipedia.org/wiki/Uniform_Resource_Locator) as my identity to be used for logging into websites such as [Facebook](https://facebook.com) or [Twitter](https://twitter.com).

Both of these sites along with numerous others allow you to use your login from another website to [authenticate](https://en.wikipedia.org/wiki/Authentication) yourself with their services. This means that I can have all my [social network](https://en.wikipedia.org/wiki/Social_network) profiles using one URL to verify my presence via. The idea being that I go to facebook, enter my blog's URL, facebook redirects me to my blog to gather my username and password which then redirects me back to facebook with an encrypted authorisation code that has no relationship to the actual username and password other than it being generated after I've confirmed who I am by logging into my blog. Hmm, I think I made that sound more complicated than it needs to be.

Basically, once I'm logged into my blog, I just give my URL to facebook/twitter/etc and they let me into their system without asking for more usernames and passwords. The problem, however, is that for some reason it's not working. Instead of authenticating me, it's sending me to an error page, and I have no idea why. Reports on the interweb suggest that there's a plugin incompatibility with the openid plugin that I'm using. I've tried disabling them all, however, and still get the error. Further reading on OAuth+OpenID and the WebHome movement is at [https://www.webhome.org/](https://www.webhome.org).

Update (29 Jun 10): Alas, I couldn't seem to get it running :-(
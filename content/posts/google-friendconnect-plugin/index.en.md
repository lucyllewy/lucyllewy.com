---
title: "Google FriendConnect Plugin"
date: 2009-03-22 18:14:19
published: true
categories: [random]
wpid: 321
---

I've just finished adding and testing a new social plugin for XYZ Network and IND-Web.com blogs. This plugin hooks into google's platform using open standards-based mechanisms to allow a user to join a blog as a commentator and have their comments posted to a variety of other sites as they post them in reply to blog entries. This allows for syndication between the popular blog networks and your blog via links back to your blog in association with the comments posted by your users which get crossposted to plaxo or twitter or orkut (for e.g.).

One thing to note, however, is that joining a site with your gmail/aol/yahoo/openid using the new plugin on our servers causes a new user to be created in the database. This means that if you have previously signed up to said site (or another in our network) then you will have two accounts. The local account takes precedence when you are signed into BOTH the local account and the gmail/aol/yahoo/openid account.
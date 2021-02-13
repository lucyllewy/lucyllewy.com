---
title: "First post using xmlrpc: &amp;#8220;how I fixed a bug..&amp;#8221;"
date: 2015-10-27 21:09:23
published: true
categories: [random, WordPress]
wpid: 391
---

I've not posted in a couple of days, so I figure I should explain what I've been up to with <span class="removed_link" title="http://ind-web.com">IND-Web.com</span>.

While playing about with windows I had a little look at Windows Live Writer, a blog editing tool made by nasty Microsnot. What actually took me by surprise, is just how useful it is to be able to write your blog posts off-line and then publish them all in one go when you get internet connectivity. Other clients are available, such as Ecto on the Mac, as the interaction with the server utilises well-known API calls in the form of "XML-RPC".

However, there appeared to be an issue with all clients for WordPress and the system we have running on ~~IND-Web.com~~. After spending about 8 hours narrowing down the problem, and `grep`ing our entire source-code-base, I came to the conclusion that the issue is with the way our servers are set up.

The IND-Web.com servers are fire-walled in such a way as to block loopback calls where an application that the user calls on our servers tries to connect back to the same server. However, after coming to this conclusion that the problem was with looping back, I could not find the code that actually did the call. Eventually after another 2.5 hours, I summoned up the courage to completely hack about the xmlrpc.php file from wpmu to determine where the problem was occurring. This lead me to discover that the `blogger_getUsersPosts` function was not actually getting fired, despite the code clearly indicating that it should.

So, where does this lead me? Well, after discussing with Deadpan110, he mentioned that wpmu tries to interoperate with WordPress gold's code without any file modifications. This little titbit got me thinking that perhaps wpmu has a file elsewhere that filters out the initial `blogger_getUsersPosts` function call and inserts it's own. This was the pay-dirt! In `wp-includesmu-{default-filters,functions}.php` files is exactly this filter call and accompanying function.

So, I thought to myself, if wpmu can filter the xmlrpc call to it's own function which then initiates a loopback call, why can't I do the same to circumvent the wpmu call and create my own function that does the same job without the loopback?

And there we have it, the solution presented itself. I have now created an mu-plugin which will overwrite wpmu's changes to the original xmlrpc code. My function is a bastard of a few of the original WordPress functions in the xmlrpc.php file. I have tested calling the resultant code in a few different clients and it appears to work just dandy.

The link below is to a copy of my mu-plugin which fixes the `blogger.getUsersBlogs` api call to `blogger_getUsersBlogs()` function in `xmlrpc.php`. just copy the php file (after removing the `.txt` suffix) to your mu-plugins folder in your WordPress Mu install:

[WordPress MU-Plugin to fix xmlrpc for blogger clients](/wp-content/uploads/2009/04/wpmublogger-getusersblogsphp.txt)

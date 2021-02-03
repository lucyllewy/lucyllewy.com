---
title: "Twitter Worm Shenanigans"
date: 2015-10-27 21:09:23
published: true
categories: [random]
wpid: 476
---

Oh dear, it looks like another high profile hack has managed to get the perpetrator a job in the "security" field. Stupid thing is that he probably knows next to nothing about security.

As [reported](https://www.theregister.co.uk/2009/04/17/twitter_worm_job/) by The Register:

> The self-confessed author of the recent Twitter worm has scored a potentially lucrative job doing security analysis and web development work...
> 
> ... \[The\] founder and chief exec of Web applications development firm exqSoft Solutions, told ABC that Mooney has accepted the job he offered, which will involve security analysis and Web development.
> 
> <cite>The Register</cite>

exqSoft have admitted that the job posting will bring them lots of publicity, but at what cost? The exqSoft founder, 24 gained his first security posting (in military intelligence) in similar circumstances. However, the worm in question is actually very primitive, and doesn't really do much other than post links to the author's, Michael "Mikeyy" Mooney, website.

The *real* crackers got to work after Mikeyy, a 17 year-old student from Brooklyn New York, went to the press further publicising the worm and his own sites.

He originally sent an email to BNO News of Brooklyn, but has since sent emails to ABC and others. The BNO news article quotes Mikeyy as having created the site which he was publicising out of "boredom" and because he "needed a way to make money". The site, ~~Stalk Daily~~, is a twitter clone and was offline when I tried to see how much of a clone it was.

This obvious attempt at publicity, however, got at least one cracker group riled up. They have since posted an email from Mikeyy's, now hacked, gmail account listing details of a large portion of Mikeyy's online life. The crackers tarred up the complete web root of Stalk Daily along with a SQL dump of it's database. This means that the privacy of anyone who naively signed up is shot to bits. The email then goes on to list stats about the virtual machine which ran the sites, including a dump of the last 50 root logins and their source.

Also dumped is an except from Mikeyy's /etc/shadow file which lists the encrypted "hash" of the user accounts on the machine. These hashes, once acquired, can be cracked by brute force.

Next up is a dump of some directory trees, mostly from /home, and finally the usernames and passwords for: DreamHost.com, VPSLink, Mikeyy's root password for the VPS, his cPanel (installed within the VPS), godaddy, another godaddy, GMail (iammikeyy@gmail.com), GMail (mikeyylolz@gmail.com), GMail (mikeyydomain@gmail.com), AOL Instant Messenger (mikeyylolz), Another AIM (AhmedShieb – is this his?), Skype (iammikeyy), buzznet (mikeyy), mac.com (iammikeyy@gmail.com), github (mikeyy). That's alot of passwords. I'll not relist them here, but they're archived for all to see in the [email](https://seclists.org/fulldisclosure/2009/Apr/0168.html) to the Full Disclosure security mailing list.
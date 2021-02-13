---
title: "Open Source. Really?"
date: 2015-10-27 21:09:23
published: true
categories: [random]
wpid: 801
---

I've looked at possible cross-platform mobile device development frameworks, and came across [Appcelerator](https://www.appcelerator.com/ "Appcelerator") a company with the product "Titanium", which ordinarily is licensed under the Apache 2.0 license. However, if at any time I decide I need extra functionality than that provided by the default product the Titanium+Plus modules such as the urban airship module are available. I almost downloaded the demo module but stopped to check out the Terms assigned by ticking the agree box to get through to the download; it is here that I became flabbergasted and appalled that a) nobody has mentioned this before, and b) that it is even allowed to happen.

Note Section 1.3(g) states that:

> \[signatory shall not nor allow a third party to\] (g) modify any open source version of Appcelerator's software source code ("Original Code") to develop a separately maintained source code program (the "Forked Software") so that such modifications are not automatically integrated with the Original Code or so that the Forked Software has features not present in the Original Code;
> 
> <cite>Appcelerator License Section 1.3(g)</cite>

And section 5.4 states that:

> Notwithstanding anything to the contrary contained in this Agreement, Sections 1.4 ("License Restrictions") ..." – I believe that is a mis-numbering of 1.3 judging by the textual title of the section – "... shall in all cases survive any expiration or termination of this Agreement.
> 
> <cite>Appcelerator License Section 5.4</cite>

I read those clauses as a _permanent_ ban on _legitimate_ open source development based upon the code previously released by Appcelerator under an open source license. This I feel is completely against the spirit of open-source and potentially a conflict of the two license grants Apache 2.0 vs Appcelerator Titanium+Plus Modules License, which would need testing in court to find which holds precedence.

As I didn't know who to contact about this flagrant betrayal of the term "Free", I figured I would post this blog entry and email the [Free Software Foundation](https://www.fsf.org "Free Software Foundation") for guidance.

<del datetime="2011-05-02">I'm still waiting on a response from the FSF.</del>

Update (19/01/2012): I failed to post this at the time, so I'm doing so now. I actually received a reply from Yoni Rabkin from the Free Software Foundation's Licensing department on the 2nd May (my original email was 13 April) with the following:

> &gt; Note Section 1.3(g) states that:  
> &gt; "\[signatory shall not nor allow a third party to\] (g) modify any open  
> &gt; source version of Appcelerator's software source code ("Original  
> &gt; Code") to develop a separately maintained source code program (the  
> &gt; "Forked Software") so that such modifications are not automatically  
> &gt; integrated with the Original Code or so that the Forked Software has  
> &gt; features not present in the Original Code;"
> 
> Indeed such a restriction renders the software non-free and therefore, of course, also GPL-incompatible.
> 
> The Apache 2.0 license is not a copyleft license and therefore permits use in proprietary software. This is one of the reasons the FSF recommends licensing your software under a strong copyleft license such as the GNU GPL. If there is copylefted software involved (with copyright holders who are not Appcelerator) then there would be a potential copyright violation to pursue, otherwise what we have is a case of misleading advertisement.
> 
> I would recommend contacting the Appcelerator people and explaining the problem; if they can make clear that they distribute both free software and proprietary software people will be able to choose to avoid the proprietary parts.
> 
> But please note that if you are into mobile development you will run into much worse problems once you attempt to actually release your software via these "app stores" as they actively support proprietary software (Google's Android, for example) and some go as far as banning free software completely (Apple's products, for example).
> 
> <cite>Yoni Rabkin – FSF Licensing Department – by eMail on 2 May 2011</cite>
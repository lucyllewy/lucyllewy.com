---
title: "GPL2 vs GPL3"
date: 2010-07-23 02:42:50
published: true
categories: [random]
wpid: 721
---

Lucy has been thinking about the GPL and it's forward provision clause.

Some projects are refusing to update their licensing from GPL version 2 to the latest version 3. The most notable of these projects would be the Linux Kernel. Linus [has stated](https://lkml.org/lkml/2006/1/25/273) previously that they believe the GPLv3 forces signing keys for DRM and other encryptions to be made available to whomever the code is furnished.

This may be a misnomer, as Bruce Perens has clarified (dead reference: https://www.linux-watch.com/news/NS9312220011.html): the GPLv3 doesn't force encryption keys to be made public, but rather forces hardware manufacturers (in his example) to put the DRM into hardware or user-mode. (Where the user-mode code would not be GPLv3 licensed.) They also states that in his opinion the GPLv2 also has this intent but didn't say it explicitly.

The issue Lucy thought they saw, however, is not with the Linux Kernel which excludes the wording suggested for the COPYING file that states "either version 2 of the License, or (at your option) any later version." Instead it would be those projects that do include said wording. Lucy thought, but now realises they are likely wrong after doing some reading of the actual contexts of the words and where they appear in the license, that a recipient of the code could choose to force the vendor/creator to comply with the latter version 3 of the GPL thereby requiring the more draconian (according to some) measures in the new license.

However, taking Bruce Perens' clarification of the way the GPLv3 is implemented, along with Linus' explanation of how the "or later versions" text works: Lucy can see that in actuality it is the recipient that must comply with the later version of the GPL, if they so-wishe, when and if they redistribute the code, be it modified or not. This means that nobody can force the vendor to upgrade his distribution to a more recent version of the GPL unless or until they do so themselves. It also means that the recipient can relicense any code she redistributes when the COPYING file includes the correct wording, but must do so in a way that complies with the wording of the newer license such as the anti-TiVo clauses for DRM.

So, if the original author using GPLv2 had NOT complied with the INTENT of the version 2 license and included TiVo-like code that could not be reimplemented by the recipient of the code, then the recipient must challenge, or at the very least RE-IMPLEMENT any TiVo-esque code so that when she RE-distributes the code thereafter, using the later GPLv3 which is her right to do so with the COPYING file text intact.

In this scenario, ideally someone in the chain would take the original author of the TiVo-esque code to task over their noncompliance with the GPL, be it version 2 or 3, as, like Bruce Perens states in his paper, the GPLv2 has the same meaning as the law stands today as the GPLv3, excepting things like the anti-patent clauses which state that any indemnification conveyed by one party to another must also be conveyed to all users of the GPLv3 licensed software.
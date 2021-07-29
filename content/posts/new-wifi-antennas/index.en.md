---
title: "New Wifi Antennas"
date: 2010-07-09 00:42:37
published: true
categories: [random]
wpid: 701
---

I've just placed an order for a pack of these "[Alfa 9dBi WiFi Booster RP-TNC High-Gain Screw-On OMNI-Directional Swivel Antenna](https://www.amazon.co.uk/gp/product/B0034DZU9Q?ie=UTF8&tag=thexyznetwork-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=B0034DZU9Q)". I will be putting them on my [Linksys by Cisco WRT54GL Wireless-G Broadband Router](https://www.amazon.co.uk/gp/product/B000ETX928?ie=UTF8&tag=thexyznetwork-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=B000ETX928) which I've got set up as a public hotspot.

I still need to tweak the iptables rules on the hotspot a tad, as the way it stands at the moment my entire network is visible to anybody connecting to the wifi hotspot. I do have firewalls on my computers, but I have other devices such as an HD HomeRun digital TV tuner and my Sky+HD box connected to the network, and I'm unsure as to their security credentials. Hopyfully I can set things up so that my entire network is blocked *except* my caching router, which is running a version of the Squid web cache on a custom gentoo Linux box.

Another thing I want to investigate is chillispot on my dd-wrt firmware allowing access via IPv6 the new addressing scheme which is due to replace all current internet addresses. I have already IPv6-enabled my LAN, but the addresses aren't forwarded onto "clients" on the hotspot yet, and won't be until I can be sure that things will carry on working correctly with the captive portal login system.

_The following is not an actual quote but my own musings on IPv6 which don't fit the flow of this entry:_

> The reason we need to switch is due to the way the current system was designed with addresses being 32Bits in length meaning that there is approximately 4billion(?) addresses available which we are fast running out of. Some reports say that early 2011 will be crunch time when the number of available IPv4 addresses is completely exhausted. IPv6 on the other hand uses 128bits for it's addresses which makes for enough addresses for every man, woman, child, dog, cat, horse to have a unique number and still only using a fraction of the vast amount available. – the actual number is nearly 34 followed by 37 zeros! or in engineer speak "3.4 multiplied by 10 to the power of 38", which is just huge!:
>
> <math><mrow><msup><mrow><mn>3.4</mn><mo>&times;</mo><mn>10</mn></mrow><mn>38</mn></msup><mo>=</mo><mn>340,000,000,000,000,000,000,000,000,000,000,000,000</mn></mrow></math>
>
> In fact, [wiki says](https://en.wikipedia.org/wiki/IPv6): "In another perspective, this is the same number of .. addresses per person as the number of atoms in a metric ton of carbon.", but that's wiki, so it could be wrong.
> 
> <cite>Dani Llewellyn</cite>

Edit: The new antennas are now installed and appear to be operational.

<script defer type="text/javascript" src="{{ '/mathml/mathml.js' | url }}"></script>

---
title: "WARNING: SCAM!"
date: 2010-06-03 21:51:12
published: true
categories: [random]
wpid: 691
---

There is a group telephoning unsuspecting computer users telling them that they have a [virus](https://en.wikipedia.org/wiki/Computer_virus)/trojan [infection](https://en.wikipedia.org/wiki/Infection) on their [PC](https://www.microsoft.com/WINDOWS). They claim that they are calling from [Microsoft](https://www.microsoft.com) and that you have sent them error reports as part of Windows Error Reporting feature that they have used to identify said virus.

To prove that they are correct in identifying a virus on your computer they tell you to:

1. open the "run" dialog from the startmenu or by pressing the windows key and the r key together.
2. type %TEMP% and click ok.
3. press ctrl+a to highlight everything in the folder window that pops up
4. press delete and tell them how many [files](https://en.wikipedia.org/wiki/Computer_file) the delete dialog asks you for confirmation about sending to the [recycle bin](https://en.wikipedia.org/wiki/Recycle_Bin_%28Windows%29).
5. they tell you that said number of files is the number of viruses in that folder.
6. next they tell you to go back to the run box and enter "cookies" (or maybe it was %cookies%, I had trouble understanding his accent).
7. in the folder that pops up they get you to perform the same action of deletion.

At this point they add the two figures together to come up with a value of the number of viruses in "just those two locations", and ask you to imagine how bad the rest of your computer must be. It is at this point that they tell you that they are going to help you our by giving you a free [license key](https://en.wikipedia.org/wiki/Product_key) to a piece of [software](https://en.wikipedia.org/wiki/Computer_software) they called "Advanced System Protector" which I would guess to be a virus itself, so-called [scare-ware](https://en.wikipedia.org/wiki/Scareware).

Once they have convinced you about the disaster of a state that your PC has got itself into they reportedly ask for banking details before they give you the license key. I didn't get that far because I hauled them over the coals stating that I will be reporting them to OFCom and the [Information Commissioner's Office](https://www.ico.org.uk/). Once I explained everything he had got wrong in his call he proceeded to call me an idiot and computer illiterate.

The information I managed to get out of him before showing my hand was the following:

You may contact them at one of the two numbers:

02081542672 OR 02081444013

The person I spoke to on the phone identified himself as "Kevin Jones" of Microsoft (he repeated the illegal misrepresentation of himself as being from Microsoft three times upon prompting).

The software that the license key purportedly unlocks was Advanced X Protector, where X was System, Email etc.

"What if they're legitimate?"
-----------------------------

I can hear some lesser knowledgeable people questioning my interpretation of the evidence and suggesting that the person on the phone could have been legitimate. So here is my own supporting evidence that contradicts theirs:

## Cookies

[Internet Explorer](https://en.wikipedia.org/wiki/Internet_Explorer) stores temporary information from websites in what is known as a "cookie". It stores each web-site's cookie(s) in a separate file, one per domain name, in the folder that %cookies% refers to. These files are completely harmless to your computer, although maybe not to your privacy as they may store personally identifiable codes that the website which set the cookie can use to tell when you re-visit their site. However, cookies are limited to the originating domain only and therefore cannot be used by a random website to identify you just because a different site has a cookie set. These files are NOT viruses, and deleting them will only annoy you as your preferences such as GMail/Hotmail's "remember me" feature will suddenly forget who you are.

## TEMP

%TEMP% is a runtime repository for files which a program needs to operate correctly while it is running, but which aren't needed long-term. Files in here may be viruses, but just because there is a file does not mean that it is definitely a virus. [McAfee](https://www.mcafee.com/) and [Norton](https://www.norton.com) virus scanners will be able to detect any viruses that may lurk in the temporary folder while still allowing other programs to maintain their necessary runtime data.

## Anti-Virus

Microsoft have their own [anti-virus](https://en.wikipedia.org/wiki/Antivirus_software) product called "[Microsoft Security Essentials](https://www.microsoft.com/en-us/download/details.aspx?id=5201)" which is free to download and use without requiring any license key-codes to be entered for Windows 7 and Vista, and included in all Windows 8, Windows 8.1, and Windows 10 installations. Therefore Microsoft would not give away free key-codes for software which "retails around £350" (as told to me by my friendly scammer).

## Microsoft Privacy Policy

The real kicker here, Microsoft have a Privacy Policy in place on their Windows Error Reporting system that [states](https://web.archive.org/web/20121010075211/http://oca.microsoft.com:80/en/dcp20.asp) under the heading "What types of information can be collected?":

> Your Internet Protocol (IP) address is also collected because you are connecting to an online service (web service) to send error reports. However, your IP address is used only to generate aggregate statistics. It is not used to identify you or contact you.Reports might unintentionally contain personal information, but **this information is not used to identify you or contact you**. For example, a report that contains a snapshot of memory might include your name, part of a document you were working on, or data that you recently submitted to a website.
> 
> <cite>Microsoft Privacy Policy \[Emphasis Mine\]</cite>
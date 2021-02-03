---
title: "Samba's Evolution &amp;#038; Virtualmin/Postfix"
date: 2014-06-19 02:37:50
published: true
categories: [sysadmin]
wpid: 71
---

A recent interview of Samba release manager Jerry Carter (25/mar/07) reveals some info on the upcoming 3.0.25 release which is currently in the RC phase of production (release candidate: code that is feature complete and packaged as final release to iron out any final bugs before the gold edition).
published: true

The result of the current round of testing prompted the Samba team to make Linux machines support disconnected log-on capability just like in Windows, Carter said. "So, for example, you can join a Linux host to a Windows domain, unplug it and go on the road and still be able to log on using your domain user account," he said.

Active Directory domain support has been extensively evolved, so that now a Linux machine will connect to the closest Domain Controller, for the site it finds itself in, instead of connecting to a possibly remote server over an expensive link.

IDMap now supports the RFC-2307 LDAP extension that is applied to an Active Directory domain by installing the Microsoft "Services For UNIX" package on a Domain Controller.

Samba will be able to better leverage information contained within Active Directory. "If the Samba host is joined to an Active Directory domain supporting UNIX schema attributes — like RFC-2307 or the SFU schema — winbind could retrieve that information from AD while mapping domain users and groups in a trusted Samba domain using the underlying Name Service Switch interface," Carter wrote in an e-mail.

While I'm not Microsoft's biggest fan, I do appreciate that Linux and UNIX must operate in a Microsoft dominated world. The Samba project brings interoperability with windows to our favourite operating systems. This can only be a good thing for everyone involved, from the Windows Wannabes™ to the Linux masses to the \[insert UNIX OS here\] die-hards.

I also read recently, in my monthly Linux Format, an interview that alluded to the possibility of a Samba-based replacement of the Name Server Switch, which is the fundamental mainstay of Linux/UNIX user-name/uid+groupname/gid mapping (plus a few other things). What this would mean to the Linux world (Linux, 'cos that's the OS I'm personally interested in) is that UIDs and user-names would all be accounted for by the Samba back-end instead of the standard shadow password suite.

Now this doesn't mean that shadow files would be obsolete, it just means that Samba would take overall control in assigning UIDs allowing for a site-wide or even enterprise-wide unique identifier for each user, instead of a different UID for each separate machine that the user wants to use.

Another great thing about WinBind (or whatever the Samba team call the technology when released) taking control of the NSS is that no matter what your source of authentication data (be it flat file, LDAP, Samba 4.0+'s AD implementation, or a true Active Directory), the system will always view it in the same way. It will also have the ability to search for all users beginning with the letter "a" etc.

This ability to search user data is essential for enterprises with thousands, if not millions, of users globally. The standard way to search user data in UNIX-land is to read every entry in the database (or flat file) one at a time until you reach the end, finally returning the results. This piecemeal approach is fine for single systems where there may be ten or so users, but it doesn't scale for the enterprise well at all.

All this interoperability leads me to the conclusion that the unices are bad at authentication (unless you go LDAP, which isn't compatible with windows at the moment), and Windows has the edge in this arena. However, the unices are far superior in other areas such as multi-user virtual-hosting environments such as that set up by the Virtualmin system that I use.

Virtualmin has the ability to store user data into an LDAP directory, however this does not include MS Active Directory systems. My ideal setup is to have Virtualmin handle adding users to an Active Directory system for the virtual-hosting environment to use, and also have the management facilities of the Windows-based management tools for the directory.

I suppose this would require someone to write a Virtualmin plug-in, or evolve the current LDAP users and groups plug-in, to support Microsoft Active Directory for adding and deleting users and groups. This, complete with the UNIX attributes afforded by the Services For UNIX add-on to Windows Server System 2000+.

In this manner winbind will automatically map the correct UIDs/GIDs to the correct users direct from the Active Directory database. All my systems will have the ability to use centralised single-sign-on for both Windows, UNIX and Linux hosts. This will mean much reduced administration on my part, while allowing for more flexibility.

The question, now, is whether Postfix supports Active Directory. Hmm. Postfix is the only facility on my system that I have doubts as to whether it will work with a single-sign-on system based on Active Directory. Postfix itself has support for LDAP maps, but does this extend to the Active Directory system? Thinking on this, I wonder if Postfix would be able to authenticate against the WinBind data that the up-coming Samba system will provide. So, update to the question:

> Does Postfix query against the NSS system or utilise the passwd/shadow files directly?

Well, I can answer the second part straight away. Postfix cannot utilise the passwd/shadow files, as it usually runs in a chrooted environment. So this suggests that the only way to query the user data is to use system calls which return the user data. I am guessing that the standard system calls would return information based on the NSS data, which would be pointing to the WinBind system which allows for off-line access.

That last point is important as, in the past, Windows has earned a reputation for being unreliable and needing lots of reboots. If the Windows system were to go down at all, be it a crash or a simple reboot, all account information will be unavailable meaning that Postfix will be stuck with the question of "do I accept all mail and bounce later, or refuse to accept all mail". The result will most likely be the latter, meaning that whenever account info is unavailable, my e-mail system will also refuse to accept mail with (worst case scenario) a 5xx error message.

SMTP errors in the 500-599 range are "hard" errors, so the server at the other end will assume that the e-mail has been addressed wrong and will bounce back to the originator or, worse, silently drop the e-mail completely. In an enterprise this lost mail is unacceptable. OK I'm not an enterprise, but I like to work to enterprise standards. Also, the sending server may cache the 5xx error for the addressed user and immediately bounce all mail destined for that user on my system without first checking that the error has been resolved (as a 500-599 error is also a "permanent" error, so the sending server thinks to itself "don't try that again").

So, hopefully, I can implement the above Virtualmin based system when the Samba team release the next edition. The technical details I hope to have covered thoroughly in this posting so that I am completely aware of the pitfalls of putting this plan into action. I will post again when I have the system up and running or, when I give up because it's too complicated for my little brain to comprehend ;-).
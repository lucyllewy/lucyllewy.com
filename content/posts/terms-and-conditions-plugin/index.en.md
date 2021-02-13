---
title: "Terms and Conditions Plugin"
date: 2015-10-27 21:09:23
published: true
categories: [random, WordPress]
wpid: 271
---

[Terms and Conditions Plugin 0.3](/wp-content/uploads/2008/12/tcs-03.zip)

I have taken the terms-and-conditions plugin written by Levi Putna, and repackaged it to version 0.3. My changes are solely to require re-acceptance of the terms and conditions if the site administrator changes the files on disk. This means that whenever you change the T&Cs, your users are re-prompted with the updated documents. When upgrading from 0.2 or below from Levi and the WordPress plugins repository, all users will have to re-accept the existing conditions due to a modification in how the accepted state is stored.

For the technical folk: I changed the user metadata storage to store the unix timestamp (number of seconds elapsed since january 1st 1970) of when the user last clicked the accept button. This timestamp is then compared to the file modification timestamp on one of the T&Cs files stored on the server. If the user's last accept time is LESS than the file modification time, then the user is prompted to re-agree.
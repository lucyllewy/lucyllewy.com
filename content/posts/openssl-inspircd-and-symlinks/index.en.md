---
title: "OpenSSL, InspIRCd and SymLinks"
date: 2011-09-01 03:51:40
published: true
categories: [sysadmin]
wpid: 811
---

I'm unaware how long this has been an issue, as it only reared its head after a restart of both my InspIRCd server and my desktop which I use to reach said server with XChat for Windows running.

I had some problems with failed handshakes from XChat for Windows when connecting to my freshly rebooted server running InspIRCd 1.2. XChat for Windows was reporting:

```
* Connection failed. Error: [336151568] error:14094410:SSL routines:SSL3_READ_BYTES:sslv3 alert handshake failure
```

My attempts to fix this problem were two-fold: I checked OpenSSL was correctly installed on my server and recompiled InspIRCd to make sure it was linking to the correct library. After restarting the server I still couldn't connect, so I moved to my second step where I verified that my saved SSL certificate from StartSSL was not corrupted by removing the file from my InspIRCd folder and replacing with a symlink to a known-good copy in a different folder.

After verifying that the known-good file is still intact by utilising the OpenSSL command line program with the following incantation, I restarted the InspIRCd daemon and tried connecting again.

```bash
openssl verify -purpose sslserver -CAfile /path/to/intermediate.pem /path/to/certificate.pem
```

Unfortunately, while the OpenSSL verify command succeeded, I still couldn't connect to the server with the same Connection failed errors from XChat for Windows. It was at this point that, after pulling some more of my hair out, I decided to reconfigure InspIRCd to look directly at the SSL files instead of the symlinks. Once I had done this I finally found a more usable error message with XChat for Windows now reporting:

```
SSL   Verify: [20] unable to get local issuer certificate
* Connection failed. Error: SSL failure
```

More hair-pulling later I have now replaced OpenSSL on my server with GNUTLS and managed to get up to XChat for Windows reporting:

```
SSL   Verify: [19] self signed certificate in certificate chain
```

I have, at least, managed to discover the cause of my issues being my recent installation of NMap for Windows which included OpenSSL libraries on the default windows search path. This caused XChat for Windows to bypass its default SSL subsystem for the OpenSSL provided by NMap. It also seems that some other people are also hair-pulling over OpenSSL and StartSSL's certificates, so at least I can take comfort that I'm not alone. Removing NMap from my default PATH fixes it on my client for the moment, but I worry now about users on Linux-based systems using OpenSSL as their provider library causing the same problems until they specifically tell their IRC client to "ignore invalid certificates" which will open a huge security hole on their system allowing for MitM (Man in the Middle) attacks.
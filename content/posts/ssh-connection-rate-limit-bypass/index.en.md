---
title: "SSH connection rate limit bypass"
date: 2014-06-24 14:33:56
published: true
categories: [sysadmin]
wpid: 861
---

I've got a client who uses a third-party for their hosting and not allowing my company to do so ourselves. A problem arose recently due to this third-party having instituted a rate-limit on TCP Connections such as SSH and HTTP along with fail2ban. Because it's a third-party, and we're not contracting them ourselves, it's difficult to get information about exactly what triggers the rate limit blocking.

Now as part of my work for this client I need to update their site from time-to-time. For this I use a scripted deployment which uses SSH and RSync to copy files to the server and move them into place. This script uses several dozen short-lived ssh requests in the space of about a minute to do its work. *You can see where I'm going here, right?* These several dozen requests are enough to cause the block to shut me out mid-way through my deployment leaving my client's site in an inconsistent state.

This is where SSH Pipelining comes into play...

## Pipelining

So SSH allows for a little-known mechanism that creates an ssh connection and then detaches from the console leaving the connection active but in the background. The relevant incantation is:

```bash
ssh -nNf -o ControlMaster=yes -o ControlPath="$HOME/.ssh/${HOST}.sock" ${USERNAME}@${HOST}
```

This isn't very useful as is, but becomes very powerful when you start to re-use the connection for a scripted operation. To run a command on the remote host through the already established connection, you would call ssh thusly:

```bash
ssh -o ControlPath="$HOME/.ssh/${HOST}.sock" ${USERNAME}@${HOST} /path/to/command
```

And the only piece left missing is rsync:

```bash
rsync -e "ssh -o ControlPath='$HOME/.ssh/${HOST}.sock'" -rz --delete local/path ${USERNAME}@${HOST}:remote/path
```

Once you're done with the connection, you can close it with:

```bash
ssh -O exit -o ControlPath="$HOME/.ssh/${HOST}.sock" ${USERNAME}@${HOST}
```
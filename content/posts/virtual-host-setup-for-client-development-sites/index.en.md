---
title: "Virtual-host setup for client development sites"
date: 2014-07-04 07:52:39
published: true
categories: [sysadmin, development, WordPress]
tags: [localhost, apache, client-work, WordPress]
wpid: 866
---

What follows is a response to many requests about how to set up a local environment of WordPress.

## The manual way

This first list is how my colleagues taught me to set up development sites on my local PC such that each client gets their own URL which maps to 127.0.0.1. This keeps WordPress happy and makes isolation simple and effective.

1. edit `/etc/hosts` (or on Windows `c:\windows\system32\drivers\etc\hosts`) and add an entry for `<client>.dan` utilising a unique top-level-domain and pointing it to 127.0.0.1.
2. create an, or copy an existing, apache virtual-host configuration for the new development domain and point it to wherever you want to keep your client's development site
3. unzip WordPress to the filesystem location you set in your apache virtual-host configuration
4. navigate to `http://<client>.dan/` and run through the WordPress installation routine

After following those 4 steps you now have a working WordPress installation under a potentially unique domain name to test your code and design as you build it.

## The automated way

So you're probably asking what's wrong with this setup given its numerous advantages? Well, consider mobile web development work where you need to test your development site on a physical handset. You could try to find some way of forcing the in-built web browser of the phone to reach `http://<client>.dan/` via your PC's Local Area Network IP Address. That is, however, time-consuming at best, and at worst will need a custom development effort into a wrapper application which you create yourself with no intention of releasing or supporting just to view an in-progress site.

Here we go, how I set up my development sites:

1. unzip WordPress into `/data/<client>/htdocs`
2. navigate to `http://<client>.dan/` and run through the WordPress installation routine

Wasn't that a lot simpler?!

## Technicalities

My way solves the problem I highlighted above of testing the client site on a mobile device because I created a resource record in the Domain Name System (DNS) which points .bang.bowlhat.net to my local PC's current IP Address. I can change this at will when I develop on a different network, and all client sites update en masse. (The record has a 5 minute Time To Live which allows for updates to happen very quickly unlike the rest of my DNS records which all take 24 hours minimum to update.) Using a DNS-based approach also means I don't need to edit my hosts file for every site I want to develop.

Apache is the second part of the equation. I get around the issue of having to create a new virtual host configuration for each client by utilising Apache's `mod-vhost_alias`, which allows for using part of the requested domain name in the path used to serve each site's files.

My one and only apache virtual host configuration file is below:

```apacheconf
<VirtualHost *:80>
        ServerAlias *.bang.bowlhat.net
        VirtualDocumentRoot /data/%1/htdocs
        <Directory /data/*/htdocs>
                AllowOverride all
                Order allow,deny
                Allow From all
                Options followsymlinks
        <Directory>
        ErrorLog logs/error_log
        CustomLog logs/access_log combined
</VirtualHost>
```

This configuration tells apache that we're serving all development sites from `/data/<client>/htdocs` where `<client>` is taken from the domain name used to reach the web server, i.e. `http://<client>.dan/`.
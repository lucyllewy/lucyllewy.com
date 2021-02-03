---
title: "Phalanger"
date: 2015-10-27 21:09:23
published: true
categories: [development]
tags: [php, dotnet]
wpid: 81
---

I've been working increasingly on Windows Vista, as my desktop system's Linux support is very minimal at the moment. The only Linux distro I've managed to get running on it so far is the \*buntu series (7.04), but the Ubuntu binary nVidia packages are missing libwfb or some such which is required for the GeForce 8800 series nVidia graphics cards to operate. This means that I'm reduced to using the desktop with the rather unimpressively performing 'nv' driver that comes with the Xorg X server.

I've been very impressed with how Windows Vista stays out of my way when I'm working, and it also allows me to run visual studio 2005 for which I have a pukka license. With VS2k5, however, I've been thus far unable to develop any PHP applications such as Deadpan110's CRIMP that I've become quite an active developer in. Enter "Phalanger" a fully .NET and mono compatible (windows only :-( though) PHP compiler.

Phalanger will take any PHP code and compile it to run natively on the .NET or Mono runtime environments. This not only increases deployment options to some hosts that refuse to run PHP but will consider a .NET compatible system, but also yields a speed increase in execution times (from request sent to result received) as the compilation has already been mostly done, and just requires the .NET JIT to finish off the compile to native code and run it.

Now this doesn't have anything to do directly with VS2k5, but available on [the Phalanger web-site](https://www.php-compiler.net) is a visual studio plug-in that enables the IDE to recognise PHP scripts, and allow PHP code-behind files for '.aspx' pages. I've not noticed any code completion support as of yet, but I'm hoping that this is being worked on for future releases of the plug-in, as syntax highlighting is already supported.

With this Phalanger product, I've taken the CRIMP code and, while only making minor tweaks to allow for IIS' inability to support url rewriting that apache is so good at, made it fully compatible with Phalanger/ASP.NET/IIS on the windows platform. I am now running my main development web-site on CRIMP using a "Windows Server 2003"-based appliance. All this is good news for CRIMP in that we can now fully support Windows/IIS-based systems whether they're running Phalanger or the original PHP ISAPI plug-in/CGI binary.
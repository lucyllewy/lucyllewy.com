---
title: "Should Perl &amp;#038; PHP just curl up and die quietly?"
date: 2015-10-27 21:09:23
published: true
categories: [development]
tags: [opinion, php, perl]
wpid: 376
---

Perl6 has been in development for a few years now, and there seems to be no sign of a public release. While I understand that the Perl developers don't want to force themselves into a fixed cycle it would be nice to know what the state of development is.

PUGS has been available for some time, which is an implementation of Perl6, but I don't know how closely it tracks the design process \[of Perl6\]. PUGS is written in haskell, due to the similarity of features available. PUGS, however, seems far from even matching Perl5 in terms of speed and capability, let alone being able to track the latest and greatest from Larry Wall et al, despite being a project started way back in 2005.

So, with there being no capable Perl6 implementation that I'm aware of, and the Perl6 developers being tight lipped and not having released their version despite having a mostly-finalised set of design schematics in 2004: will Perl6 *ever* be released to the wide world?

It is this lack of obvious movement that leads me to question whether Perl6 is still viable when there are other projects that seem to have stolen Perl\* users. I'm thinking here about Python, Ruby and PHP.

It would seem that many system utilities that would have been written in Perl "back in the day" are now being written in Python; while Ruby and PHP are battling it out for the Web Crown which was once held by Perl with CGI Scripting. Both Python and Ruby are fully objectified, while Perl continues to provide headaches to anyone trying to use object-oriented design patterns. PHP still hasn't got the namespace thing down, but it's object orientation isn't *too* bad, however Ruby seems to be increasingly stealing PHP's web thunder like Python with Perl in the System Utilities department.

Is Python the harbinger of death for Perl6?

Is Ruby the same for PHP? Or will newbies always gravitate to PHP for it's deceptive easyness. Which brings me on to another issue: PHP is too easy! PHP encourages new programmers to not "sanitise" inputs from web forms, and therefore expose themselves to the multitude of internet-bourne attacks. The problem is that even long-time PHP "developers" still won't have picked up the skills necessary to mitigate these attacks, as exemplified by PHPBB (the forum system).
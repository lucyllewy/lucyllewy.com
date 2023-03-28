---
title: "PHP and the isset() function"
date: 2007-09-17 13:50:34
published: true
categories: [development ]
tags: [php, oop, best-practice]
wpid: 101
---

It has been common usage to utilise the `isset()` function to determine if a variable in PHP has a value. This is as opposed to checking the contents are not `NULL` or the empty string: `""`. I had utilised this facility thoroughly in the CRIMP code-base (available on source-forge under the same name), and was wondering why a certain scenario wasn't being handled as I expected. Now, I knew I could work around the issue, but I felt I needed to find out what the actual cause was.

As it turns out, as I was using PHP5's class infrastructure and pre-declaring variables class-wide, the `isset()` function was always returning true. This meant that my checks for the existence of an object within the `$crimp->_debug` variable was always telling me that the object existed when, in fact, the code hadn't got as far as initialising the debug routines yet. This was also true for my checks on the `$crimp->_config` variable which houses the configuration array.

This latter issue, the `_config` variable, meant that if the config.xml file was not present or was unreadable then CRIMP would return a blank empty html document to the browser. The reason was that CRIMP was detecting failure to load the configuration file, and was trying to parse and send an error document to explain the fact. However, because the `_debug` and `_config` objects hadn't been initialised yet, some of the calls made in the error page parsing routine were against objects that didn't exist.

The solution was to replace the `isset()` calls in my routines with the more appropriate `if ($this->_config != "")`. It seems to me that many newbie errors can be eliminated by explaining that `isset()` only checks the variable's existence, and that it should always be validated by checking the contents of the variable also. In my opinion, the `isset()` should never be used on it's own, as all it is useful for is avoiding PHP Notices about missing variables.

So, my advice to newbies in the field of PHP programming is:

> Don't assume that because a variable is defined (and isset() returns true) that it has the value you are expecting. In other words, check, check and, when you're sure the variable contains what you expect, check again.
> 
> A simple check for the correct value doesn't eat too many computer cycles, so it is better that you put checks in place, and use up a couple of milliseconds, rather than tear your hair out for days trying to work out why it's not working.
> 
> <cite>Lucy Llewellyn</cite>
---
title: "BBC's Botnet"
date: 2009-04-19 00:42:38
published: true
categories: [random]
wpid: 471
---

The BBC recently (14–16 March) repeatedly ran a show, 5 times in total, under the "click" banner on the BBC News channel highlighting the dangers of the so-called "botnet". Excerpts from the show are [here](https://news.bbc.co.uk/1/hi/programmes/click_online/7932816.stm "BBC Click Botnet Show (Excerpt)"), [here](https://news.bbc.co.uk/1/hi/programmes/click_online/7938201.stm "Another excerpt from BBC Click's botnet experiment show") and finally [here](https://news.bbc.co.uk/1/hi/technology/7938949.stm "Final excerpt from BBC Click's BotNet Experiment"). What they did is paid some malware hackers for access to their botnet, and then recorded what they did with it. Two experiments they ran were sending of spam emails to specially set up email accounts, and a Distributed Denial of Service (DDoS) attack on a security company's ([PrevX](https://www.prevx.com/)) backup site (after gaining permission to do so).

[Mark Perrow](https://www.bbc.co.uk/blogs/theeditors/mark_perrow/) of click [said](https://www.bbc.co.uk/blogs/theeditors/2009/03/click_botnet_experiment.html "Mark Perrow's blog about the BBC Click BotNet Experiment") after questions were raised as to the legality of this experiment:

> In seeking to demonstrate the threat, had we put ourselves in the position of those we wanted to expose? ...
> 
> ... we could have simply described what we believe happens and given some warning advice, couldn't we? ...
> 
> ... But hacking has gone professional. Today, your PC can be doing bad things to other people without you even knowing. ...
> 
> So we felt that there was the strongest public interest in not just describing what malware can do, but actually showing it in action. A real demonstration of the power of today's botnets – to infect, disrupt and damage our digital lives.
> 
> <cite>Mark Perrow</cite>

I tend to agree with Mark on this point, but others like [IT Security Expert](https://www.itsecurityexpert.co.uk/)'s Dave Whitelegg disagree:

> I'm ALL for raising awareness of cybercriminal activities, but I think BBC Click programme crossed the ethical line on this one, in they actually used a botnet (namely thousands of PCs infected with centrally controlled malware) without the PC owner's permission to send out Spam Emails.
> 
> <cite>Dave Whitelegg</cite>

Which aren't considered spam, because they were wanted. The definition of Spam is "unsolicited" (and possibly 'commercial') "mass mailings". The important word there is "unsolicited". The BBC controlled both the sending software and the recipient email accounts, so therefore the emails were not unsolicited.

Dave goes on to say:

> Furthermore I am troubled the BBC paid criminals thousands of pounds of license payer's money to buy the botnet. I think they were ill-advised to take this course of action...
> 
> ...For instance I would consider it highly un-ethical to purchase stolen payment card details from a cybercriminal...
> 
> <cite>Dave Whitelegg</cite>

Now, I'm sure that Dave was thorough in his research, but I must inform him that the BBC has done exactly that (purchased stolen credit card and identity information). I can't seem to find a reference now that I hunt for it, but I'm fairly sure I saw it on either an episode of Panorama or "Inside Out" (possibly from the south – where I live – or otherwise from a country-wide roundup episode).

[The Register](https://www.theregister.co.uk/2009/03/12/bbc_botnet_probe/) remains fairly fence-straddled in it's reporting of the incident. They spoke to Sophos' senior technology consultant Graham Cluley:

> \[he\] added that changing people's desktop wallpaper to present a message from BBC Click ... clearly crossed the line. "Even if it was done with the best intentions and in the public interest, that is unauthorised modification of a computer and an offence under the Computer Misuse Act,"...
> 
> <cite>Graham Cluley</cite>

"The computer security industry has found itself in the situation before where it has been able to do this (or remove malware on a botnet-infected computer) without permission of the computer user (after all, we don't know where they are based in the world) but has NOT because it's illegal for us to change people's computers without their permission."

Well where does this end? It could be argued that excess registry entries created by a software installer, even though legitimate, are without express permission. Surely if you were worried about following the letter of the law then you must prompt the user on *any* change to ask whether it is permissible or not. Moving onto the security industry's issue of being unable to "change people's computers without their permission", then any real-time malware scanner must *not* clean an infection it encounters due to the lack of evidentiary permission.

The register also quotes Struan Robertson, editor of [out-law.com](https://out-law.com/).

> "The \[Computer Misuse\] Act requires that a computer has been made to perform a function with intent to secure access to any program or data on the computer. Using the botnet to send an email is likely to satisfy that requirement..."
> 
> <cite>Struan Robertson</cite>

Sending email does not fit the requirement of securing access to any program or data on the computer itself. Think of it this way: the bot on the target computer does not access any other file to perform it's task of sending an email, and therefore is not trying to secure access to said other file. If I were on a jury, I would say based on that description of the act that more evidence needs to be presented to convict.

Also, changing the wallpaper is by means of entering new data into the system and therefore is not altering anything within the system except one registry key which tells the computer where to find the wallpaper file. Altering or Modifying requires there to be data which is largely intact after said modification. In no terms do either of the experts state anything about creating a file or deleting a file, though the latter could be termed a modification in that the file's index entry is modified to remove the system's knowledge that the file exists (which it still does, even though it doesn't get listed).
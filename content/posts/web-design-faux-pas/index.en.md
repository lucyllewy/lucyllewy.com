---
title: "Web Design Faux Pas"
date: 2009-04-14 01:18:06
published: true
categories: [random]
wpid: 456
---

I was reading through the [Ten Most Violated Homepage Design Guidelines](https://www.useit.com/alertbox/20031110.html) by Jakob Nielsen and one item struck my eyes as being important for the WordPress designers, along with designers of other CMS systems such as Joomla!, to take stock of. Namely "10. Don't include an active link to the homepage on the homepage".

It is very common for people to use the default widgets that come with WordPress for listing links to their pages and important articles. However, I have noticed that these widgets do *not* mask-off the current page and include it as a link. This results in the user being unsure as to whether they are in the right place or whether they need to click again, and again, and again.

Quote from the article:

> Active links to current pages cause three problems:
> 
> – If they click it, a link leading to the current page is an utter waste of users' time.
> 
> – Worse, such links cause users to doubt whether they're really at the location they think they're at.
> 
> – Worst of all, if users do follow these no-op links they'll be confused as to their new location, particularly if the page is scrolled back to the top.
> 
> <cite>Jakob Nielsen – [Ten Most Violated Homepage Design Guidelines](https://www.useit.com/alertbox/20031110.html)</cite>

The article states that these links usually come from a universal navigation bar, as I alluded to above. I think WordPress needs to have a function available which you pass two variables:

1. The page URL which should appear in the link – this could come from other functions such as `the_permalink()`.
2. The text which should be wrapped up inside the link if the user is not on said page. If the user *is* on the same page, the text should be output with no modification, or maybe with `<em/>` tags surrounding it so as to highlight the fact that the text indicates the current page.

I have utilised the guidelines listed in the aforementioned article to aid me in designing a [new site](https://rllewellyn.co.uk/ "R. Llewellyn Electrical Contractors") for my brother's business. (The site is not live at the time of writing, but if you're reading this in the relative future then try clicking the link.)
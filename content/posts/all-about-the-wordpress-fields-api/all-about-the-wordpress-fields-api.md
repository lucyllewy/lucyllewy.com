---
title: "All about the WordPress Fields API"
date: 2016-01-19 16:17:01
published: true
categories: [development, WordPress]
wpid: 1233
---

You have probably heard about the effort lead by Scott Clark with guidance from Helen Hou-Sandi to develop a WordPress Feature Plugin mysteriously entitled "WordPress Fields API". So what is it, what does it do, and why should you care?

Feature Plugins
---------------

First-up we need to understand what a Feature Plugin is in relation to WordPress Core:

When the WordPress community decided that we needed a major rewrite of the Administration screens it was realised that such an invasive change would be difficult to develop within Core's source-code repository. The reasoning is that regular releases of WordPress are likely to be required before the ~~MP6~~ (as the admin revamp was called) code was ready for the World.

As an upshot of this dichotomy between regular releases and long-term development of code that is likely to be broken many times before it's ready, the plan was concocted to do the development of MP6 as a plugin. As this plugin was planned to be merged into WordPress Core when it became ready it was dubbed a "feature plugin". Thus the concept of Feature Plugins was born, and now most larger development efforts are worked-on in this manner.

What is the Fields API then?
----------------------------

Now we know what a Feature Plugin is, we can explore what the Fields API is:

As you've probably guessed, and I alluded to above, the Fields API is being developed as a Feature Plugin. The idea behind this feature is to provide an API that WordPress and plugins & themes can utilise to add arbitrary data structures to every object that WordPress handles. These objects include posts, both from the in-built post-types and custom post types, navigation menu items, comments, users, and settings.

Why do I care?
--------------

While it may not be something you interact with knowingly, the Fields API should pervade everything you do with WordPress no matter whether you're a visitor, member, author, or administrator. Every time you enter data into a WordPress site we hope that this will be via a form-field created, output, and saved, by the Fields API.

Plugins and themes will be able to use the API to replace all their custom solutions for form-fields. A previous attempt at unifying some of the admin area settings pages was implemented which we call the Settings API. However, this is only part of the battle, and we anticipate that the Fields API will serve as the back-end to the Settings API in addition to being used elsewhere by non-settings-related forms.

When can I get it?
------------------

Thus far a small dedicated team including myself and lead by Scott have been working on getting a proof-of-concept out of the door for user-profile screens. This effort is nearing readiness but there will still be a lot of work to complete once this POC is done. Right now the feature plugin isn't ready for testing but we hope that it will be soon. If you're interested in helping-out with development you can join us on the WordPress Slack channel #core-fields where several people hang-out and discuss plans. The [source-code for the WordPress Fields API Feature Plugin](https://github.com/sc0ttkclark/WordPress-fields-api) is hosted at github under Scott's account; please Fork the code, comment on or create issues in the tracker, fix bugs, and file Pull Requests if you spot anything you can help with or can advise improvements or talking-points about.
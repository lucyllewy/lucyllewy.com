---
title: "TimThumb EOL (finally!)"
date: 2015-11-23 18:52:03
published: true
categories: [random, WordPress]
wpid: 1170
---

The Make WordPress blog has finally signalled the end of a disastrous piece of software that made many WordPress sites insecure. TimThumb is/was supposed to allow embedding of images at any dimensions without requiring WordPress to previously have created the variant. The project has been used by many, many, themes available on commercial sites such as ThemeForest and also quite a few that have been published in WordPress.org's repositories.

[TimThumb EOL](https://make.WordPress.org/plugins/2015/11/19/timthumb-eol/)

The problem is that the project has been plagued by insecurity after vulnerability after blatant holes ever since it launched. Thankfully WordPress.org has signalled that their hosted repositories will no longer allow TimThumb to be included in plugins or themes uploaded from now-on. That means **new** plugins and themes cannot be added with the code, but still allows existing plugins and themes to have a period of respite as the plugin isn't being *retroactively* banned just yet.

The move, in my opinion, is long overdue but at least it's done now and we can all move forward using more standardised methods such as the in-built `add_image_size` functionality that Core provides out of the box.

[`add_image_size()`](https://developer.WordPress.org/reference/functions/add_image_size/)

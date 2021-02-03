---
title: "ezCache"
date: 2015-10-27 21:09:23
published: true
categories: [development, WordPress]
tags: [WordPress, caching]
wpid: 131
---

This page is no-longer relevant.

—

After spending a while looking into caching methods on [WordPress](https://www.WordPress.org "WordPress"), Deadpan110 and I have started work on a way to reduce database server load.

Most caching methods for WordPress involve creating static ([WP-Cache](https://WordPress.org/extend/plugins-cachecom/ "WP-Cache is an extremely efficient WordPress page caching system")) or semi-static ([Staticize Reloaded](https://WordPress.org/plugins/staticize-reloaded/ "Staticize Reloaded is a plugin to make your site faster by caching the output of some WordPress pages.")) pages and these are very good at reducing web and database load but are very quirky on sites with a lot of dynamic content.

Although the database cache does not reduce web server load, it does reduce the amount of queries sent to the database server.

At the time of this posting, my [home page](/ "bowlhat.net") performs 38 queries to the database server.

With database caching enabled, 21 queries were retrieved from the cache and only 17 queries were made to the database server.

Even with the cache timeout set to the minimum of 1 hour, the load on a database server can be dramatically reduced on busy sites.

The WordPress database class (WPDB) is a modified version of an earlier release of [ezSQL](https://web.archive.org/web/20111023003920/http://www.woyano.com:80/jv/ezsql "ezSQL is a class that makes it ridiculously easy to use mySQL, Oracle8, SQLite and others within your PHP script") and can be easily replaced by creating your own database class in `.-content/db.php` which is the same method the WordPress site uses for its [HyperDB](https://WordPress.org/plugins/hyperdb/ "HyperDB is a replacement for the standard WPDB class").

> HyperDB is a replacement for the standard WPDB class which adds the ability to use multiple databases.
> 
> <cite>HyperDB</cite>

The cache I have created just drops into the `wp-content` directory and extends the current version of ezSQL for use with WordPress.

### Features

- Uses the cache feature within ezSQL
- No caching of WordPress options, admin pages or previews
- No caching of any SQL that makes use of **RAND**
- Cache timeout can be set to a minimum of 1 hour (default)
- In case of emergency, the cache can be switched off or even made to revert back to the WordPress default wpdb class from within `wp-config.php`

The script and documentation will be available soon – watch this space...

In the mean time, you can see the cache statistics at the bottom of these pages.

The project pages for this were available in [the ezCache section on Deadpan110's site](https://web.archive.org/web/20100916064407/http://www.deadpan110.com:80/WordPress-plugins/ezcache/).
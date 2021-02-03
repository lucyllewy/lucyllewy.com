---
title: "wp-cron"
date: 2008-11-11 02:09:11
published: true
categories: [random, WordPress]
wpid: 236
---

Martin of our sister company has written a nice how-to for setting up WordPress cron jobs to run properly when using services similar to our own, which prevent loop-back access to the same machine. The way that WordPress usually runs, is that when a user clicks onto your site after the scheduled time for cron to be fired has gone past the engine opens a loop-back http request to itself. This is to prevent needlessly locking up the server leaving the user staring at a blank screen while ping-backs are fired before they see the page.

This means that when services are configured like XYZ Internet's and IND Web's servers, the wp-cron just times out and doesn't fire any ping-backs or do any other maintenance commands. The solution is to add a system scheduled command which uses the command-line php interpreter to call the wp-cron page directly. XYZ Internet and IND Web's system allows 3 of these scheduled commands, so the cron can be fired once every 20 minutes.
---
title: "Twitodon - Find your Twitter friends on Mastodon"
date: 2022-04-29T09:00:00+01:00
draft: false
categories: [development]
tags: [mastodon, twitter, migration, twitocalypse, Elon Musk]
---

## The Twitocalypse
When it comes to the news in April 2022 about Twitter agreeing to be purchased by Elon Musk for $44bn, and placed into private ownership, the social network has been abuzz with fear for the future of the platform and its moderation policies. Many people are [joining Mastodon](https://joinmastodon.org/) to hedge their bets against a potential Twitocalypse. The problem is that there has been no way to map users between Twitter and Mastodon making it hard to discover your friends when they move across. In the past there was the Mastodon Bridge that filled this need, but it has long been defunct leaving a vacuum.

## Introducing Twitodon

To alleviate this problem I have spent the past 48 hours writing a replacement for the Mastodon Bridge entirely of my own design and implementation. The process is simple:

- Navigate to my [Twitodon website](https://twitodon.com/)
- Click the Login With Twitter link and authorize the application
- Click the Login With Mastodon link and again authorize the application
- Finally, click the "Click Here to download the CSV file" link to download a CSV file that can be imported to your Mastodon account.

## The CSV File

The CSV file will contain a list of every Mastodon user that Twitodon knows about. It maps users that have been to Twitodon and authenticated with both Twitter and Mastodon. The caveat is that even if your friends are already on Mastodon, if they don't visit Twitodon and login with Twitter and Mastodon then they are undiscoverable by Twitodon. Twitodon *requires* that as many Twitter to Mastodon refugees create the link between their Twitter and Mastodon accounts as possible to ensure that when more refugees arrive they're more likely to discover their friends who have already made the move.

## Getting updates

Twitodon does not monitor for updates. Once you've downloaded your CSV I advise that you schedule some future date (maybe a month, or six months from when you last downloaded the CSV; although as the service is very new there are very few mappings in the database, so it might be prudent to come back more regularly until the service is more widely known and there are many more users in the database) to download an updated CSV file.

## Sponsor me

If you find the service useful, please consider donating via [GitHub Sponsors](https://github.com/sponsors/diddledani) to help me pay for what might become quite an expensive-to-run service.
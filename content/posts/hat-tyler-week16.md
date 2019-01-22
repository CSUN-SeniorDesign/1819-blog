---
title: "Tyler's Blog. Week 16."
description: "New Year, New Projects."
date: "2019-1-25"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 16"]
---

#### What's new this week?
This is the first week back after Winter break. Since last semester ended, there were a couple decisions that were revealed over winter break. Our client chose not to pursue using AWS, and instead wants to use SiteGround for their web hosting.

As a result, the AWS infrastructure we worked on from the second half of last semester will no longer be used. This week, we are gathering more information from the CS team about how they are pursing the new changes, and gathering log-in information for SiteGround.

We will be aiding the CS team in the transition from AWS to SiteGround, although we don't believe there is a way to use our GitLab CI-CD Pipeline with SiteGround. We didn't get login information to the siteground site until Thursday, so there isn't much we could do for them this week. The following section will go more into detail about the SiteGround platform and functionality.


#### SiteGround Research
Siteground provides Web Hosting, WordPress hosting and WooCommerce Hosting. Our client, Crista chose to use the WooCommerce hosting for her website.

The basic WooCommerce hosting allows for a Drag & Drop builder, unlimited data transfer, 10GB of storage with an included MySQL DB, sub-domains and cPanel.

The platform also includes Cloudflare CDN and a Static content cache with automatic daily backups (but a higher tier must be purchased to restore from backup).

The most interesting part of the platform is included SSH and SFTP access. This factor can allow for at least some automation to the website. I believe it's also possible to install git (but it isn't pre-installed unless a higher tier is purchased.) It's also noteworthy that a Staging site isn't available unless a higher tier is purchased.

Overall, SiteGround seems like a relatively simple website hosting platform. The main advantages are the Drag & Drop builder, CDN, Caching and Auto-Backup. The regular price for the standard tier is $11.95 a month, which seems overpriced for the hosting provided, as something similar could be accomplished using AWS Lightsail, which is around $3.50 a month. 

Lightsail provides a WordPress App + OS solution, but the plugins will have to be installed manually. AWS Lightsail can also be connected to AWS CDN for an additional charge, or Cloudflare CDN.

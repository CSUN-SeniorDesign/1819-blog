---
title: "Hat-Hayden-Week-26"
author: Hayden
date: 2019-04-12T04:36:35-07:00
tags: ["Hayden Wilcox", "HAT", "Week 26"]
draft: false
---

Overview:
- Continued working on and looking into Cloudfront CDN

What I did:
- Though I had gotten the CDN to work with my own private bucket, it still needed to function with our Wordpress site. 
- I began researching into how I could make the static bucket work with the dynamically loading site.

Issues:
- Upon consulting our teammates, I realized I had a critical flaw in my thinking: the Wordpress site is dynamically generated when a user loads it.
   - This means that my original idea of storing all site content in the bucket had to change.

Next Time:
- We ultimately decided to follow a model similar to the way Discord, a chat service handles it. Though the chat boxes are dynamically generated based off of an html file, pictures and videos are stored in a bucket. In doing this, Discord now links to the static content in the bucket rather than it's original source. Doing this for our Wordpress site would help with the loading times of our posts.
- Discord also utilizes Cloudflare, which our new system design does as well. I will be looking into Cloudflare and how to make it work for our site and purposes.
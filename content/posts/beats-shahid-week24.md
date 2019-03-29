---
title: "Shahid's Blog. Week 24."
date: 2018-3-29T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 24"]
layout: 'posts'
---

## Shahid's Blog. Week 24
#### What's new this week?
This week I worked on separating the development version of our web application from the production version. This included setting the environment variable NODE_ENV to 'production' and using the Helmet middleware NodeJS module to protect against http header vulnerabilities.

Currently I am using a service file and systemd to start the web application whenever the EC2 instance starts. I have tried using forever NodeJS module in order to auto start the web application but it is not working.

In the following week, I will stop using Cloudflare for HTTPS and will instead use Certbot to apply HTTPS on our web application. I will also switch to PM2 in order to start our web application.

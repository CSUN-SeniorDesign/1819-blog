---
title: "Hat-Hayden-Week-25"
author: Hayden
date: 2019-04-05T13:04:45-07:00
tags: ["Hayden Wilcox", "HAT", "Week 25"]
draft: false
---

Overview:
- Continuing from last week, I created a module for Cloudfront CDN.

What I did:
- Created the module for Cloudfront CDN. This involved having a main.tf file that set up the structure of the module. This included a lot of the backend work that wouldn't needed to be altered. 
   - IPv6 is allowed on the CDN, HTTP traffic is redirected to HTTPS for security, and I restricted AWS Edge coverage to just the United States and Canada. The last point was done to save money, but also as our blog really does not need edge coverage for the entire world. Though we are no longer working with Child Safe Legal, the company would not have served people outside of the US either
   - The only things the user really needs to change is what bucket they want the CDN to utilize. They can also specify which file will be loaded when they access the url provided by the CDN. In my case, the index.html file I created last time.
- Created a bucket policy that made the S3 bucket listed publicly accessible. Though this is insecure, we ultimately will only be hosting static content in the bucket with no sensitive information such as passwords. This was deemed the simplest and most time efficient method, which is why I decided to go this route.

Issues:
- The structure of the module was very dense. A Cloudfront CDN requires many attributes to be addressed correctly in the module in order to work properly.
- Originally, I was going to make an Origin Access ID that would allow access into the bucket. This would require connections to first sync up with that ID in order to access bucket content. However, this proved to be a complicated and confusing process that would require me to spend a lot of time researching and testing. It was decided on making the buckets public in order to avoid this issue.

Next time:
- I will need to document my module and prepare for the presentation on Monday. Afterwards, I will look over the sprint plans with my team and decide which aspect I will work on next.
---
title: "Hat-Hayden-Week-24"
author: Hayden
date: 2019-03-29T12:35:36-07:00
tags: ["Hayden Wilcox", "HAT", "Week 24"]
draft: false
---
Overview:
- My assignment this week was to set up our Amazon Cloudfront CDN. Cloudfront is a service that increases the speed of a site's ability to load content through the usage of caching. Amazon has locations all over the world. Our S3 bucket located in California would take some time to be loaded in a decently far away country such as Britain. However, with Cloudfront, the first time a website is accessed by anyone in the country, it gets cached in the nearest Amazon "Edge" location. From now on, anyone who attempts to access the site from that country will have their load times much improved, as it will be accessed from the cache as opposed to the bucket itself in another country.

What I did:
- Using the AWS console, I set up a bucket with a simple html file inside, and a png logo that would be loaded with it. Then I setup a Cloudfront distribution that utilized the bucket. The distribution provided a URL that I went to to check that everything was being loaded properly from the bucket.

Issues:
- Due to the amount of data to propagate, even a small bucket takes a distribution a long time to deploy, approximately 5 minutes.
- I could not access the content inside the bucket, leading to an error page when I attempted to load the URL. I later discovered that this was because while the bucket could be publicly accessed, the objects inside the bucket could not. Once the proper permissions were set, the html and png could be loaded without issue.

Next Time:
- As it stands, I did everything through the AWS console. This was just to test the software, so that I got a feel for what I was actually setting up and what it did. Over the course of next week, I'm planning on creating a module for the distribution process so that one is created and deployed when we run terraform apply.

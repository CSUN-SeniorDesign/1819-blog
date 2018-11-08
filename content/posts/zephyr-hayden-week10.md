---
title: "Project5p1 Hayden"
author: "Hayden"
cover: "/img/cover.jpg"
tags: ["Hayden Wilcox", "Zephyr", "Week 10"]
date: 2018-11-02T14:47:29-07:00
draft: false
---

For the week of Project 5, we were implementing the plan we created last week for Project 4. While reviewing the plan we made during the last project, we discovered that there was a cheaper solution to what we were going to be implementing. Our first plan was to have a more dynamic system, utilizing an elastic load balancer and an EC2 instance to host our website. However, we realized that by cutting these out and focusing the system around our S3 buckets, the site would become more static, but would be much cheaper to maintain. 

I did not have direct contact with the site being made since my other team members handled the construction and implementation of the new site. It was decided that the work was easy enough that not every member needed to contribute directly. I was however, tasked with updating the design document from Project 4 to reflect the design decisions we made after review. I also created another diagram reflecting the new design of the project.

To explain my diagram, we set up a Route53 which associates our domain name with our website, and directs traffic from the internet to our S3 bucket where the content is kept. The Cloudfront Delivery Network (CDN) makes sure that the latest content is loaded up and delivered to the user. The content is delivered to the S3 buckets from the GitHub using CircleCI which allows us to constantly update the content directly.



---
title: "Project5 Post Neel"
author: "Tonny Wong"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 10"]
date: 2018-11-02T21:54:48-07:00
draft: false
---

For this project we had to implement project 4 
where we had to find the cheapest way to host our website. 
All the team members helped looking for the cheapest way to 
build for our blog in Project 4 and based on our pervious project 
knowledge we decided to go with circle and s3 bucket. 
We also had ALB and EC2 instance that we were going to do but it 
was a lot expansive.So we decided to drop the use of EC2 instances
and everything involved such as the ALB. 
This significantly cut down the cost of hosting our site from around 
70 dollars per month to maybe 3 dollars using our new design. 
The new way we are trying to do includes a Domain that uses Route 53 
to point to our S3 buckets that host the contents. 
We would use CircleCI to push the static files to the zephry90.com s3 bucket. 
We also Implemented an extra feature such as Cloudfront to hand and push our 
content to people who were accessing our site.

The main issues I had was working with cloudfront using terraform. 
The settings were not quietly working for me and sometimes the certification 
was not being included. After looking at the console and doing 
it manually I was able to figure out the settings I needed to set in terraform. 
We took parts of the old Terraform files to only get information we need for this project.

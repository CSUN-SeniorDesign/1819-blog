---
title: "HAT-Hayden-Blog-13"
author: "Hayden"
date: 2018-11-23T17:32:03-08:00
tags: ["Hayden Wilcox", "Zephyr", "Week 13"]
draft: false
---

This week was rather slow for me in terms of work getting done due to the holiday season. Reviewing what still needed to be done, I realized that the last thing that needed to be done from one of our GitLab issues that I had worked on, was to create an elastic IP. I created this manually to be utilized later. I was then going to work on both the ALB and ASG, when I realized that Aubrey had already completed the ALB in full. This left me to work on just the ASG.

The first order of business was to set up the launch configuration. I followed some of the previous documentation written on the subject by my previous group to help proceedings. I used a basic Linux AMI and a T2.micro setup for cost-effectiveness. For the time being, I am using the security group I had previously set up for the NAT instance earlier. This is subject to change in the future, depending on what connections we want to allow. After this, I created the ASG, using this launch configuration and the private subnets created last week. It has a min size of 1 and a max size of 2, but this may also change in the future if large amounts of traffic are expected.
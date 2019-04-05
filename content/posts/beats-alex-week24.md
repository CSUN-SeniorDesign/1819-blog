---
title: "Alex's Blog Week 24."
date: 2019-03-29T21:55:36-07:00
draft: false
layout: 'posts'
tags: ["Alex Enaceanu  ", "BEATS", "Week 24"]
---
# CIT 481 - SAPS
#### Week 24
I haven't gotten fully back into the loop since I was in Asia for spring break. I also got sick on my way back, so I have been resting a lot hoping I'm not patient zero of some new flu strain. I was unable to attend the Monday presentation, but I have met with my team and reviewed all the work they have completed. This past few weeks was about changing from Fargate to EC2 since we were being charged credits for Fargate usage. Some of the things that were required to address for this endeavor are the launch  
config, CloudWatch alarms, and the details of the Autoscaling group. I will be working on documentation for the AutoScaling group.

The team is also implementing Grafana for infrastructure monitoring. Grafana is like other monitoring solutions. It has a dashboard where you can view desired metrics. Of course, Grafana must be installed on an EC2 instance that is running Amazon Linux in our case. It is a straightforward installprocess that requires ssh access and certain ports to be open.

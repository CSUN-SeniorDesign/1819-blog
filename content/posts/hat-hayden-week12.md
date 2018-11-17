---
title: "HAT-Hayden-Blog-12"
author: "Hayden"
date: 2018-11-16T16:16:41-08:00
tags: ["Hayden Wilcox", "Zephyr", "Week 11"]
draft: false
---

My major task for this week was to configure the baseline for connections to and from our website. My first order of business was to set up the VPC. I looked up the steps from my previous projects to help me out. I created the VPC with a Class B IP range with /16 subnetting (172.31.0.0/16). I then created the internet gateway, and moved on to the subnets. I created two public and two private subnets using a range of 172.31.1.0/26 - 172.31.1.64/26 and had them located in US-West-2 for the availability zone. We were originally only going to create one of each subnet, but we decided to have secondary ones to future-proof the system in case we needed them in the future.

After finishing with the subnets, I moved onto the NAT instance. At this time, I began to discuss more thoroughly with my team mates on what we wanted to do for the infrastructure. We decided that our main goal above all else was to try to reduce costs wherever possible. As such, while I was orignally going to create a NAT instance and a Bastion instance, it was decided that we would create one instance that fulfilled the function of both, to have one less instance to have to keep track of and pay for. After creating a basic instance, I created a security group for it, which opened up ports for SSH, TCP and UDP connections amongst others. Then finally I created route table associations to link all of the subnets with their respective route tables, as well as making a route table specifically for the NAT instance.

Around this time, I presented my work to my team mates and asked for their assistance. Aubrey was the first to answer, adding a route table that I had missed, and fixing some typos. He also suggested that I create security groups for the future ALB, and any potential private testing instances that we might want to create. I agreed and these were added as well.
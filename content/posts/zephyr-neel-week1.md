---
title: "First Post - Neel Patel"
author: "Neel Patel"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 1"]
date: 2018-08-30T11:45:49-07:00
draft: false
---

Neel Patel:<br>

Beginning of project, first thing I create was an organization for AWS to link all four AWS accounts of 
team members into one single AWS account. By putting all the account into an organization, we are able to spend 
$500 credits instead of single $100 credit we as student get from AWS Educate. After that, I purchased a domain (Zephyr90.com) 
from godaddy since I was familiar with it. After that started working on creating the Virtual Private Cloud (VPC) 
which create a virtual network for us and to install instances inside that VPC. As a default, AWS has a VPC already 
created to help customer, but since we wanted to use the CIDR block I deleted the default VPC and created a new one 
with the same CIDR block (172.31.0.0/16) and giving the name Zephyrvpc. After creating the VPC, I created an internet 
gateway so that in future; if I install an instance it can then communicate with outside world using this gateway. 
After creating the gateway (Zephyrgw) I attached it to the VPC. After that I broke down the 172.31.0.0/16 network into 
subnetworks; three public subnet in three different zone with 1024 available host; three private subnet in three different
zone with 4096 available host. The public subnet are /22 to for 1024 hosts and the private subnet are /20 for 4096 hosts. 
After the subnets were created, I had to create route for both public subnet and private subnet in route tables. 
For public subnet, I had to add a new route 0.0.0.0/0 so that any ip address accessing the instance could communicate 
through the gateway that was created. Since the private subnet are private, it should only accessible from within the 
network so we did not need to add any route. After that, I associated the public subnets to the public route and private 
subnets to private route.



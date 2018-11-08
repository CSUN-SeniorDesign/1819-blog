---
title: "Second Post - Neel Patel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 2"]
date: 2018-09-06T19:02:38Z
draft: false
---

<br>
For second week of project, I was tasked to install Nginx web server on our EC2 instance created by Hayden. For Nginx to communicate through HTTP/HTTPS I had to add few rules into the firewall-ufw so that communication between internet and web server is possible. After installing Nginx I started working on creating a tutorial documentation that goes through how to set up a VPC in AWS, how to create public subnets and private subnets; how to create route tables, how to associate the subnets into appropriate route table, and how to add a new rule in public route able so that communication from outside is accepted. As second week started, we were having problems with our Hugo files on the Ubuntu since at first they were created on windows and had settings that pointed files to wrong directory. So when we tried to copy and run our Hugo server we got errors stating that files do not exists and some files were missing. Therefore, to fix the issue Tonny installed Hugo on to the EC2 Ubuntu 16.04 instance and reposted the posts to make sure everything looks ok before I had to turn those Hugo file into .html static files, which later were deployed through Nginx to our domain zephyr90.com. As individual I had to logged into the instance using the private ssh key provided by Hayden to go into the Hugo site we had created and add my second week post and run the Hugo server to see if the post looks fine. After other team member enters all the second week posts, I had to recreate static files and deploy it again on the domain.

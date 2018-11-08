---
title: "Second Post - Hayden Wilcox"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Hayden Wilcox", "Zephyr", "Week 2"]
date: 2018-09-05T18:42:57Z
draft: false
---
<br>
For Week 2 of Project 0, I was assigned the task of setting up the Elastic Cloud Computer (EC2) server for our website. I have had experience setting up Linux and Ubuntu virtual machines on Oracle VM in the past, but never on AWS. Thankfully though, the procedures were mostly the same and intuitive. I kept most things default but changed a few key features to suit the needs of our group’s project. I used the T2.micro instance type as it was free and the current scope of our project does not require any stronger processing. Neel was in charge of setting up the VPC and subnets, so when it came time to make the Ubuntu instance, I used these features. This was actually a subject that I needed to learn, as I did not know what the Virtual Private Cloud was or why we needed it. I learned that it was what allowed our group to connect to our server from anywhere and do work on it. When it came time to add security groups, I noted that SSH is available by default, and then added HTTP, HTTPS and also opened up port 1313. HTTP/S were instantiated to allow other clients to connect to our website domain, while port 1313 allows us to conduct testing on the Hugo website without committing changes to the domain unless we’re sure we want to. After the setup of the EC2 was done, it had me generate a pair of private and pubic keys to allow me to securely log in to the server. It gave the files to me in .pem format so I used PuttyGen to convert it to .ppk format. I had not used PuttyGen before either but thankfully it was also fairly intuitive. I generated three more sets of keys for my teammates to use when logging into server, and added them to .ssh/authorized_keys so they could be checked on login to make sure the connection is secure and no unauthorized individuals can access the site.


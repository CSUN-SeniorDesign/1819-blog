---
title: "Irrigation Post 1"
author: "Daniel Hirahoka"
cover: /img/cover.jpg
tags: ["Daniel", "Zephyr", "Week 11"]
date: 2018-11-10T14:48:30-08:00
draft: false
---

# Week 11

For week 11 we have been trying to keep in contact with the Software and Hardware development team that have been tasked the Irrigation system. The whole team consists of 14 members 6 which are on the Hardware and 6 who are on the Software and 2 for the CIT. While both the Hardware and Software team have been trying to get a basic understanding of the requirements they are going to need for the system we have been trying to also figure out what they need from us. After speaking with both teams and a small presentation by a man named Art we have some understanding on what they need.

One the main objectives for the Software team is creating a website to act as a hub for the irrigation system. We don't know exactly what they are going to have on the site as of now but we know it will be some sort of dashboard and we can try to get a site like that running.
Accompanying the site will include two Relational Database, one for logins and the other helps manage the sprinklers. Since our team isn't very familiar with Databases this is something we are going to be looking into but it seems that are choices are limited. Since we have concluded that the database has to be a relational one there isn't many options on AWS other than Amazons Relational Database (RDS).

This is our official task as of now, while they get an understanding of what needs to be done we have to setup the database and website using Terraform and CircleCI.

# The Other Project : Encryption System
On top of our duties to assist the irrigation system we have a sub task of helping out an encryption team with their work. Fortunately for them we don't have a lot on our plate from the Irrigation project so we are allowed to help them as long as we have the time too. They have explained to our team the project they are doing and what they expect from us as of now.

This includes creating s3 buckets for the files they are trying to store, possibly creating EC2 instances to host servers and containers that have need to have certain software installed on them. As well as databases for logins and possibly other databases in the future. All this plus a testing environment so they can see if they stuff they do works or not using AWS.

We are currently working on setting up the testing environment so they can get familiar with it as soon as possible.

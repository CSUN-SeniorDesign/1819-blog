---
title: "Sandbox Worms Yerden Week25"
date: 2019-04-05T23:30:46-07:00
draft: false
Categories: [AWS]
Tags: ["Yerden Zhursinbek", "Sandbox Worms", "Week 25"]
---

This week I continued researching on bastion host. So, bastion hosts are instances that sit within your public subnet and are typically accessed using SSH.  After it connected remotely with the bastion host, it acts as a jump server, allowing you to use SSH into other instances within your VPC. There are 2 main pieces invloved. Ansible configuration file and and ssh config file.

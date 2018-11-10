---
title: "Thomas Week 11"
date: 2018-11-10T08:27:21-08:00
layout: 'posts'
draft: false
---

# Week 11
For week 11 I met up with my group and we figured out how to divide ourselves into subgroups to help divide up our tasks. Taking a look at all we have to accomplish for Advancing Tech until next May, we figured out what we wanted to tackle first for the next 2 weeks. We decided that we wanted to start with setting up the firewalls and DHCP and gave one of those tasks for each group. My group was assigned to figure out the current firewall configuration, setup any additional features and automate its setup and configuration.

## VPN and SSH
In order to connect to the machines from outside the CSUN network we would need to setup a VPN client on our personal machines to VPN into CSUN's network so we can SSH into each machine. I was able to download the Pulse Secure software on my laptop and had no issues connecting to CSUN's network through VPN. After this I could use my terminal to SSH into the machines using the credentials provided to us. Using the VPN only applies when we are working from home away from CSUN's network. When on CSUN's network we can SSH directly.

## Firewall
Once we had access to each firewall machine we could begin to figure out how the firewall was setup and if anything would need to be added or changed. For now we have been able to look at the configuration using iptables and have also been able to determine that each firewall is running on Ubuntu 18.04. Our next task is figuring out how exactly the current configuration is functioning and documenting that information. After that we can begin figuring out what needs to be added or changed and how we can automate the setup and transfer of the firewall configuration between machines. 
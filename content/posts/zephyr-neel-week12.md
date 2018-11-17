---
title: "Project5 Post Neel"
author: "Neel Patel"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 12"]
date: 2018-11-02T21:54:48-07:00
draft: false
---

For this week for the Advance tech lab we started documenting all the 
setting that we found in the servers that are currently up and running.
We also try to understand more than 250 lines of IPTABLES rules which
I have minimal knowledge about. I was happy to learn more about Ubuntu
in general, since I usually work with Windows Operating systems at work
but as i researched more into the rules things were getting more and more confusing.
Also starting this week, we were not able to ssh into our Ubuntu 18.04 server which is meant for VPN.
To fully understand the configuration, we decided to practice using Virtual Machine.
So we created two Virtual Machines that will act as our firewalls and later 
we would need one for VPN and others for testing purpose for the Private Network.
Also since we could not ssh into our what would be our VPN server we asked the professor to
ask professor Wiegley since I knew he said something about ordering new machines and replacing
them, but it turns out to be a routing issue. The issue was that in our firewall A default route was
not configured so the packets were being sent in but not being sent back, but the issue is solved using 
route command to add a new default gateway route. Also we were trying to understand how the 
network is set up so that when we practice in Virtual box we know how the NIC are configured with ip address 
and one thing I found was that in earlier version of Ubuntu we configured the NIC using the /etc/network/interfaces, 
but in Ubuntu 18.04 the NIC are configured using Netplan /etc/Netplan/*.yaml which uses a YAML file to configure the Network cards.


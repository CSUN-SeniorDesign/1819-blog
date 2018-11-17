---
title: "Matabit Thomas Week 12"
date: 2018-11-16T15:59:06-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 12"]
draft: false
---

# Week 12
For week 12, our team took a deeper look at the network configuration and firewall settings that is currently set up so we can get a better idea of what we're working with and what still needs to be done. 

## Network configuration
In order to get a better idea of the network configuration, we connected to firewall A via ssh and ran a simple ifconfig command to take a look at its network adapters. After running the command we could see that there were four network adapters currently installed - two public and two private. 

eth0 - 130.166.240.19
eth1 - 130.166.47.2
eth2 - 10.47.0.2
eth3 - 10.0.47.2
ib0 - 192.168.47.2

We were able to determine that Eth0 is the public subnet that contains both of the firewall machines A and B. Eth1 is the other public subnet which contains the VPN which will allow access to the private subnets. Eth2 is the private subnet that contains the servers in our network. Eth3 is a private subnet that allows communication between each firewall. Ib0 is a special adapter set up for use with InfiniBand. These network adapters are set up in a similar fashion on firewall B with the exception of the InfiniBand adapter which is not present.

## IPtables
The iptables file contains the rules by which the firewall acts upon packets. These rules can specify source IPs, destination IPs, and protocols and can approve or deny packets based on these parameters. To determine the rules currently listed we used the command: sudo iptables -L -n. This commands lists all the rules and chains applied and shows IP addresses and protocols in number format. Currently there are over 250 rules listed across many different chains. One of our goals for the coming weeks will be to analyze these rules further and make sense of them. We will see which rules are necessary and which need to be adjusted or added. Once we figure this out we can automate these rules between firewalls.
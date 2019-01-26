---
title: "Matabit Thomas Week 13"
date: 2018-11-26T22:30:07-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 13"]
draft: false
---

# Week 13
For week 13 I began to setup virtual machines on my local machine using virtual box to mimic the set up of the actual servers that are going to be used. We are setting up virtual machines as a group so that we can practice our configuration of the services and settings without having to worry about messing up our actual servers as we learn what needs to be done.

## Virtual box
Using virtual box we set up three virtual machines each running Ubuntu 18.04. Two of the machines will represent firewalls A and B and the other will represent a server. Each virtual machine has three network adapters set up. Two network adapters are bridged which allows them to communicate with the outside world as well as other virtual machines privately and the other is set up as an internal network. The bridged network adapters are representative of the public subnets the firewalls are located on and the internal network represents the private subnet the internal servers are located on.

## What's next
The next thing we need to figure out is how to configure both DHCP and iptables rules on our firewall virtual machines so that they can assign IP addresses to the virtual machine representing servers on the private subnet and also monitor traffic going in and out of the network and drop packets that aren't approved. We need to figure out these configurations so that we can apply the same settings on the actual firewall machines. We have lost a lot of time because of the holiday weekend but we will begin working on figuring out the configuration of the firewalls for the coming week.
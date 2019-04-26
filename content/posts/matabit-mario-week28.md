---
title: "Matabit Mario Week28"
date: 2019-04-26T15:11:12-07:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 28"]
layout: 'posts'
---
# Week 28

This week we worked more on the iptable rules and fail-over services. For the iptables there are a series of commands add that add configurations to the rules already existing on the servers. It saves the rules and flushes the previous rule sets to apply the new exceptions. 

### Corosync and Pacemaker
Corosync is an open source program that provides cluster membership and messaging capabilities, often referred to as the messaging layer, to client servers. Pacemaker is an open source cluster resource manager (CRM), a system that coordinates resources and services that are managed and made highly available by a cluster. In essence, Corosync enables servers to communicate as a cluster, while Pacemaker provides the ability to control how the cluster behaves. We will be using these two services to replace the current fail-over service keepalived we have installed on the servers. 

Thursday, we worked on a side project for Lisa. She wanted us to install Ubuntu on one of the servers and remove the password on the bios. First thing we did was remove the password on the bios by simply using a breaker located on the motherboard that resets and removes the password. The next step was configuring the 4 drives by setting up a raid 5. During the process we discovered one of the drives was failing so we had to remove it and later replace it with another drive which with a raid 5 is simple as the new drive will simply rebuild itself. The next step was installing Ubuntu server on it. We had several issue with installing Ubuntu 18.4 and 18.10 as during the installation it asks to configure the network which was not working, so we decided to install a older version of Ubuntu 16 which installed with no issue. However once we got into the server we were having networking issues that prevented us from pinging other locations. We need to look further into the networking configurations to figure out the issues we are having. 

#### What's Next?
Next week we plan on installing and configuring Corosync and pacemaker, complete the firewalls with fwbuilder, and fix the ILO connection issues. 
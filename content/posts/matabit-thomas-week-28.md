---
title: "Matabit Thomas Week 28"
date: 2019-04-27T00:19:45-07:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 28"]
draft: false
---

# Week 28
Starting off week 28, we were still having issues connecting via https/http on old firewall A which was preventing me from cloning our repo with the personal access token as well as performing an apt-get update on the machine. Given that every other machine had no issues connecting on these ports, we came to conclusion that it must be an issue with the firewall settings. At first I believed that I figured out the problem when I noticed in fwbuilder that rule 22 was negating https/http traffic to the firewall cluster. However, when we un-negated this rule and compiled the new iptables, we still coulnd't apt-get update or clone the repo. So some other rule must have been affecting this type of traffic before even reaching rule 22. So what we ended up doing was flushing our iptables and starting over fresh. So we were able to perform what we needed to but there are now no iptables rules filtering any sort of traffic to and from the machine. So the rules will have to be rebuilt using fwbuilder and tested along the way to determine where issues were being caused for https/http.

We also accomplished some tasks for a side project involving installing ubuntu on a new server. Initially, we removed the BIOS password by switching the password jumper on the motherboard and was able to set up the three harddrives in RAID 5 configuration, given that the fourth harddrive ending up failing. After this, we attempted to install ubuntu 18.04 LTS from a USB but were not able to get past the network configuration segment of the set up because we couldn't find a network port on either of the switches that would allow us to complete the set up. We ended up installing ubuntu 16.04 on Lisa's recommendation because it didn't require a live network to complete the setup. What we'll have to work on next is getting the network set up properly so we can update the OS and install services such as openssh-server so that people can securely login to the machine.
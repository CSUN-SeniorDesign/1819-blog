---
title: "Matabit Thomas Week 22"
date: 2019-03-08T20:51:40-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 22"]
draft: false
---

# Week 22
For week 22, the advancing tech group continued to transfer and test services on the new servers so that we can eventually shut off the old servers. 

John and I attempted to install FWBuilder on the new Firewall A but were running into a problem where the machine couldn't find the package to install. Upon further inspection I noticed that the machine could not ping domain names. Attempting to ping google.com brought up nothing but pinging google's IP address: 8.8.8.8 worked perfectly fine. This led me to believe that something was wrong with the new server's DNS resolving settings. Taking a look at the resolv.conf file, the nameserver is saved as 127.0.0.53. We changed this to 8.8.8.8 and we were able to resolve domain names on the server again and could ping google.com no problem. We took this chance to install FWBuilder on the server and had no issues opening the GUI using SSH with X11 enabled. We were able to open the firewall configuration files just fine using FWBuilder so we would be able to use the software to generate the proper iptables. However, resolv.conf eventually defaulted back to the original nameserver of 127.0.0.53. This is something we'll have to take a deeper look at to determine what about that IP address is preventing us from resolving domain names. The settings are the exact same in the old servers but those can ping domain names no problem.

I continued to work on my own to make sure the backup scripts are working the way they should. On my own virtual machine, I set up the script to back up the actual DNS, DHCP, IPTables, and netplan config files rather than just pretend configuration files like I was testing on earlier. Everything worked as expected however it wouldn't copy over a couple files due to a permissions issue. Those two files were: /etc/dhcp/ddns-keys and /etc/bind/rndc.key. I will have to take another look at this so we don't run into this issue when we deploy the scripts on the live servers. 

Other than that I worked on the ansible project that was assigned in class. I didn't get any experience with ansible last semester so it was a bit of a new experience. After reading through some documentation I discovered that it is a much more simple tool than I originally thought and is really useful for remotely setting up machines with identical configurations. I learned quite a bit about the syntax and how to set up playbooks as a series of different tasks to be run sequentially. Running the playbook against localhost was simple enough and I could verify that everything worked the way I wanted it to. I would like to dive more into learning how to use ansible on remote hosts.
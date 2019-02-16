---
title: "Matabit Mario Week19"
date: 2019-02-15T23:10:26-08:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 19"]
layout: 'posts'
---

# Week 19 - DHCP Config Files 

This weeks main focus was on the migration from the old servers to the new servers. We had modified the DHCP configuration file to work for our new servers. The old configuration files had the mac addresses for the old servers. In order for that configuration file to work with the new servers it needs to have the same mac addresses as the new servers. We left the old servers configurations files along with the new servers configuration file. The reason for that is if for any reason the old servers need to be up and running again all that has to be done is  to uncomment the mac addresses for the older servers and they would be up and running with no problem.  

### Issue 
One issue we ran across was transferring over the DNS and DHCP files using SCP.
The issue was transferring over the files due to permission errors. We had to specify the user host and path and access the file owned by root so we have higher access in order to move over the files. 
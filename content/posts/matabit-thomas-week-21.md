---
title: "Matabit Thomas Week 21"
date: 2019-03-01T16:25:51-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 21"]
draft: false
---

# Week 21
This week consisted of making sure DHCP is working properly on the new servers so that when we power off the old ones there won't be any networking issues. This involved using netplan to edit the .yaml file that is being used to configure the network interfaces. We were able to use it to hard code the IP addresses of the two new firewall servers. We also used nmap commands to get a better look at the networks and the hosts attached to each one. Right now all the ports on the servers are working the way they should with the exception of one on the new firewall B. 

Mark informed me that it would be a good idea to back up the netplan .yaml file as part of our scripted backups along with the DHCP, DNS, and firewall configurations. I have added additional lines to the scripts to allow a backup and a version creation for this file and I will push the changes to gitlab after I have properly tested it. Once the two new servers are up and running we will be able to clone our repo to them and start deploying our scripts to create backups of our configuration file. Right now we are thinking of creating an account on gitlab called "Techlab" which we will use to add the ssh keys and begin pushing versions from.
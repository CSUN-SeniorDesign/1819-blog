---
title: "Matabit Mario Week 21"
date: 2019-03-01T14:59:24-08:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 21"]
layout: 'posts'
---
# Week 21 

This week consisted mostly of doing excessive research on our servers to get a better understanding of our network. We used nmap a lot to map out the network to modify the config.yml file for our netplan configuration to change according to the proper network interfaces. For our dhcp file we hard coded the IP addresses according to the appropriate interfaces. That way once we shut off the old firewalls the new firewalls take over place of the old firewalls. We used the netplan configurations settings to assign the same IP’s for the new servers. One issue we kept having was when we changed the IP address in the net plan yaml file and an error would occur and the machine would go down and we would no longer have access to the machine. We would then have to ssh into the new firewall or ilo to restart the system. As of now the last thing we accomplished was hard coding the new IP’s into the netplan yaml file and were able to establish connection to a network and ping them. Before the day ended we headed over to the MDF to work on the network and get the ports up and running. An email was sent to the IT network guys to get the 130.166.240.x ports up and running. Currently we have all the ports on the servers working except for one port on New Firewall B - enp4s0f0 10.0.47.212. 

Our next step is to finalize the networking addresses once the IT guys get the ports we need up and running. 
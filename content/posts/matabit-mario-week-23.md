---
title: "Matabit Mario Week 23"
date: 2019-03-15T14:49:32-07:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 23"]
layout: 'posts'
---

# Week 23

This we was a pretty tough one working on the servers. We struggled with several things that the solution was right in front of us. However, after good communication and long hours of work we were able to push through and open a new window. First off I started by pinging google.com and googles IP 8.8.8.8 to see if our DNS resolving is working properly. The ping worked with the domain name but not the IP address. Which got us thinking the dhcpd.conf file needs to be looked at and modified in order to find out the issue. After doing a little research I thought our firewalls are on but blocking port 53. That did not turn our to be the case, we were able to ping CSUNs and Facebooks domain name and IP addresses and get back successful results. This left us thinking that the issue is from googles IP address 8.8.8.8 and has nothing to do with out servers. The next step we moved onto was testing new A and B servers to see if we can ssh into them directly without the need for old A and B. So we turned off old A and B and attempted to ssh into new A and B. The ssh was successful, however this could mean that the servers are just floating and available to the public and not part of the network we want. Meaning the firewalls are just there and being used. After further research we are now able to access the new firewalls directly from outside using the .21 and .21 interfaces. Since we can now access both firewalls on the interfaces we want, we can now work on turning off the old firewalls. We well be keeping an eye out on DNS resolving to see if an issue arises. We also want to turn off old services and have them taken over by the new firewalls. 

### Next Week?
Next week is spring break, we plan on meeting couple of days out of the week to work on the servers as we do have a presentation the following Monday. The plan is to turn off the old firewalls completely and work on having the new servers take over. 

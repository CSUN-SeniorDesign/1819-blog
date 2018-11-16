---
title: "Matabit Mario Week 12"
date: 2018-11-15T21:43:28-08:00
draft: false
tags: ["Mario Kassab", "Matabit", "Week 12"]
layout: 'posts'
---

# Week 12 

For week 12 me and my team continued to work on Firewalls A and B to find out more information about the servers and how we are going to go about automating/configuring them. 

We used ifconfig to find all the newtork adapters on both firewalls.

```
Eth0 - 130.166.240.19/29 (public subnet that contains firewalls)
Eth1 - 130.166.47.2/24 (public subnet that contains VPN)
Eth2 - 10.47.0.2/16 (private subnet containing servers)
Eth3 - 10.0.47.2/24 (private subnet bridging firewall machines)
```

Firewall A has an additional adapter for InfiniBand.
Ib0 - 192.168.47.2

## Netplan 

I also learned about a new tool that we are going to be using to configure interfaces which is Netplan.

Netplan is a utility for easily configuring networking on a linux system. You simply create a YAML description of the required network interfaces and what each should be configured to do. From this description Netplan will generate all the necessary configuration for your chosen renderer tool.

## iptables

We used the iptables command:

``` sudo iptables -L -n```

This lists every chain and formats IP addresses and ports as numeric output.

## VirtualBox

The final task we setup was virtualbox on our personal machines to clone the firewalls we are working with so we can practice on. This way we can configure the firewalls any way we want and not have to worry about compromising anything. Once we figure out how to set up the firewalls and configure them, we will start working on the real firewalls. 

## What's Next?

For next week, we plan to start working on the real firewalls and finish setting up the automation/configuration. 
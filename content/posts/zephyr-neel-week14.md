---
title: "Zephyr Neel Week 14"
date: 2018-11-30T14:57:18-08:00
draft: false
---

	For this week of the Advanced Tech Team 1, I was trying to understand how the network 
	is designed and the configurations so that I can mimic it using the Virtual Box. 
	So to start I created two virtual machines both Ubuntu 18.04 where one would be the 
	Firewall/Router and other would be a client for testing purpose. 
	To start I added two network cards to the firewall/router where one would be NAT, 
	which will act as our WAN and other will be internal network, which will be our private network 10.47.0.0/16. 
	The way it was supposed to be where the clients on private network will get their internet access from the NAT(WAN) network interface. 
	To start I first had to enable ip forwarding to start the project by editing the 
		/proc/sys/net/ipv4/ip_forward and changing 0 to 1 to allow ip forwarding. 
	After that I had to edit the interfaces with ip address and one thing I had to learn in Ubuntu 18.04 
	was Netplan since it handles all the network interface configuration. Since I the WAN comes with ip I had to 
	let it get ip from a DHCP, but for the Internal network I had to give it a static ip address by editing the 
	/etc/Netplan/50-cloud-init.yaml and adding enp0s8 interface with ip of 10.47.0.1/16. 
	After saving the file I ran Netplan apply command to apply the setting and rebooted the system. 
	After that I had to add iptables rules to allow internal network access to internet via the WAN network interface. 
	The rules I added were: 
		iptables --table nat --append POSTROUTING --out-interface enp0s3 -j MASQUERADE
		iptables --append FORWARD --in-interface enp0s8 -j ACCEPT
	After this to test I went started the client Ubuntu 18.04 and edited the /etc/Netplan/50-cloud-init.yaml file and 
	adding enp0s8 interface with ip of 10.47.0.2/16 so that it’s in the same network as the router/firewall. 
	After I pinged the router ip 10.47.0.1 and got a reply. Also one problem I had was that after I rebooted the router/firewall 
	for updates the iptables rules were gone so to fix the problem I save them using the iptables-save > /home/techlab/iptablerules.fw and 
	edited the /etc/rc.local. I added 
		#!/bin/bash
		/sbin/iptables-restore < /home/techlab/iptablerules.fw
	This will load the iptables rules on boot everytime, but after error and fixing I had to change the file permission using 
	chmod 744 /etc/rc.local to give execute permission. I will be doing more iptables rules and understanding the network little better.

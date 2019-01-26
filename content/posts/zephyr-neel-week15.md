---
title: "Zephyr Neel Week 15"
date: 2018-11-30T14:57:18-08:00
draft: false
---

	For this week of the Advanced Tech Team 1, We begin by looking at the new two servers that Mark had gotten from
	the professor to install into the racks. The servers have three hard drives ~ 256 GB = ~450 GB. Since we only had 
	three out the four supported drives we had to decide about the RAID configuration. So we had RAID 0,also known as a 
	stripe set or striped volume, which splits data evenly across two or more disks, without parity information, redundancy, 
	or fault tolerance. RAID 1 on the other hand consists of an exact copy/mirror of a set of data on two or more disks; 
	a classic RAID 1 mirrored pair contains two disks and the drives will run fine as long as one is running. Since we
	had three drives we decided that we would install our operating system,Ubuntu 18.04 Server install, in on of the drives and mirror
	the two so that whatever important configuration we do and programs we install will operate as long as one drive is working.
	Since Mark had the server i decided to try changing the RAID configuration in a server since i have never done it and also install the
	operating system. After that i saw that the server had four network interface like the one we are configuring in our Virtualbox virtual
	environment. Like the professor had mentioned at the end of the presentation that since we are working in an environment where ips can 
	be changing consistently we have to set static ip on the important hardware that we know so that first the IP will always remain same and does not change
	since if the ip changes and you have no way of know then it we have to wait until someone else tell us. So i went ahead and did static ip on the testing machines i am running.
	Example:
				host fwb-hb {
				  hardware ethernet 00:11:0a:5b:15:f3;
				  fixed-address 10.0.47.3;
				}
				host fwb-lo {
				  hardware ethernet 00:17:08:51:75:7a;
				  fixed-address 10.47.255.98;
				}
	We are currently looking at all the dhcp files so that we can create automation for the second firewall so when it comes up it automatically pick up the DHCP configuration.
	After i am going to try installing VPN on the the my testing VPN server to match the professor requirements.
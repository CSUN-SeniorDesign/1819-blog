---
title: "Matabit Mario Week13"
date: 2018-11-25T19:20:11-08:00
draft: false
layout: 'posts'
tags: ["Mario Kassab", "Matabit", "Week 13"]
---

# Week 13

Due to the upcoming holidays, the team and I were left with less amount of days to meet up and work on the project and discuss further on what our next issues are. This weeks main focus was to take a further look into the web server and take a look at config files to see what we need and what we don't need in order to make the adjustments to the config files when we decided to implement and automate them. One thing we did cover as a team is mapping of the network in order to get a better understand of how these machines are communicating with one another. By running the ifconfig command on each firewall, I was able to determine the main machines and their IP addresses.

## DHCP.conf file 

I was able to find the DHCP.conf file and take a look at the contents in order to get a better understanding of how to duplicate it on my own virtual machine for practice. 

First, I had to install the dhcp using the install command 
```sudo apt install isc-dhcp-server``` then, I had to configure the file according to our scenario using the vi command ```sudo vi /etc/dhcp/dhcpd.conf ```

The file should look as so:

```
option domain-name "tecmint.lan";
option domain-name-servers ns1.tecmint.lan, ns2.tecmint.lan;
default-lease-time 3600; 
max-lease-time 7200;
authoritative;
``` 
this sets up the global parameters.

```
subnet 192.168.10.0 netmask 255.255.255.0 {
        option routers                  192.168.10.1;
        option subnet-mask              255.255.255.0;
        option domain-search            "tecmint.lan";
        option domain-name-servers      192.168.10.1;
        range   192.168.10.10   192.168.10.100;
        range   192.168.10.110   192.168.10.200;
}
```
this defines a sub network. 

```
host centos-node {
	 hardware ethernet 00:f0:m4:6y:89:0g;
	 fixed-address 192.168.10.105;
 }

host fedora-node {
	 hardware ethernet 00:4g:8h:13:8h:3a;
	 fixed-address 192.168.10.106;
 }
 ```
 this Configures Static IP on DHCP Client Machine.

 the final step was to save it and start up the DHCP. 

 ## What's next?
For next week, I hope we get further into the project and at least start automating some stuff. Communication between the team has been a little tough but I am sure it is due to the holidays. 
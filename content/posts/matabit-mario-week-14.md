---
title: "Matabit Mario Week 14"
date: 2018-11-30T18:24:49-08:00
draft: false
tags: ["Mario Kassab", "Matabit", "Week 14"]
---

# Week 14

For week 14 I mostly worked on setting up and installing dhcp and DNS on my virtual machine to copy the servers we are working on for our senior design project to practice on. I had to configure the dhcp conf file according to our scenario. 

## DHCP 

For dhcp I first had to install it using the command ```sudo apt-get install isc-dhcp-server``` and then upgrade and update using the command ```sudo apt-get update && sudo apt-get upgrade```. After I installed dhcp and update the files I had to edit the dhcpd.conf file ```Sudo nano /etc/dhcp/dhcpd.conf```.

The dhcpd.conf file looks as so:

```
option domain-name-servers 10.47.0.1;
default-lease-time 3600;
max-lease-time 7200;

ddns-update-style none;

authorative;

subnet 10.47.0.0 netmask 255.255.0.0 {
    range 10.47.0.6 10.47.255.250;

    option routers 10.47.0.1;
    option brodcast-address 10.47.255.255;
}
```

After the file was configured I had to restart the service using the command ```sudo service isc-dhcp-server restart status```.

The next step was declare what interface the dhcp is to serve requests on ```sudo nano /etc/default/isc-dhcp-server```.

The file looks as so:

```
INTERFACESv4="enp0s8"
INTERFACESv6="eth0"
```
Next I had to edit the sysctl.conf to enable packet forwarding for IPv4

```
net.ipv4.ip_forward=1
```

## DNS

For DNS I had to install Bind9 on firewall/router, this can be easily done by running the command ```sudo apt-get install bind9```. 

The second step was to configure client for new DNS server, ```sudo nano /etc/resolve.conf```

```
Nameserver 10.47.0.1
```

## What's Next? 
For next week, me and my team are going to continue to work on setting up the servers on our virtual machines. We also be working on our final presentation for the semester. 
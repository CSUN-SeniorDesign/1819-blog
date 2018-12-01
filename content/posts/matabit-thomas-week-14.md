---
title: "Matabit Thomas Week 14"
date: 2018-11-30T22:43:56-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 14"]
draft: false
---

# Week 14
For week 14 we continued to configure our virtual machines to mimic the actual network and service configurations set up by Professor Wiegley. This included setting up IP forwarding, creating network interfaces with netplan, adding some basic iptable rules for testing, enabling DHCP to allocate IP addresses to our private machines, and establishing DNS for our private machines to have internet access.

## IP forwarding
To enable IP forwarding we edited the following file with the command sudo nano /proc/sys/net/ipv4/ip_forward and changed the 0 to a 1.

We also used sudo nano /etc/sysctl.conf and uncommented the line that read net.ipv4.ip_forward=1.

## Netplan
Netplan allows us to create a description of our network interfaces in a file and generate those interfaces in the backend and apply them to our system. The following code is used in our netplan configuration file to create our network interfaces:
```
network:
    ethernets:
        enp0s3:
            addresses: []
            dhcp4: true
        enp0s8:
            addresses: [10.47.0.1/16]
    version: 2
```
From there you can generate and apply this configuration using:

```
sudo netplan generate

sudo netplan apply
```

## iptables
We used the following commands to add iptables rules to test out our firewall capabilities:

```
sudo iptables --table nat --append POSTROUTING --out-interface enp0s3 -j MASQUERADE

sudo iptables --append FORWARD --in-interface enp0s8 -j ACCEPT

sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

In order to save our iptables rules and apply them again after a system reboot we had to save the rules to a file and use a bash script to load them in again on boot up. 

```
sudo iptables-save > /home/techlab/iptablerules.fw

sudo nano /etc/rc.local

#!/bin/bash
/sbin/iptables-restore < /home/techlab/iptablerules.fw

sudo chmod 755 /etc/rc.local
```

## DHCP
Install DHCP server:
```
sudo apt-get install isc-dhcp-server

sudo apt-get update && sudo apt-get upgrade
```

We edited our DHCP configuration file to enable DHCP on our firewall machine.

1. Uncommented #authoritative
2. Edited domain-name-servers to 10.47.0.1
3. Edited the subnet:
```
subnet 10.47.0.0 netmask 255.255.0.0 {
    range 10.47.0.6 10.47.255.250;
    option routers 10.47.0.1;
    option broadcast-address 10.47.255.255;
}
```
Lastly we edited what network interface our DHCP server was on.
```
sudo nano /etc/default/isc-dhcp-server

INTERFACESv4="enp0s8"
```

## DNS
To enable DNS on our firewall machine we needed to install bind9:
```
sudo apt-get install bind9
```

After installing bind9 we needed to point our client machine to our new DNS server:
```
sudo nano /etc/resolv.conf

nameserver 10.47.0.1
```
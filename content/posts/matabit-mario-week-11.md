---
title: "Matabit Mario Week 11"
date: 2018-11-09T17:40:52-08:00
draft: false
tags: ["Mario Kassab", "Matabit", "Week 11"]
layout: 'posts'
---

# Week 11

For week 11, I met with the others members of the Advanced Tech project. We divided the teams into groups of two, one team has 4 members and the other team has 3 members. Each team got assigned a project to work on. For my team we were assigned to work on the Firewall/Router, the other team was in charge of working on the DNS/DHCP. There was not much work for us to do with the firewall besides access the existing firewalls and routers and completely document the current
installation so we know what we are working with. We also had to document the current routing configuration for the different networks and document the firewall rules. 

## Firewalls A and B

The first thing we had to do was to ssh into the techlab server then from there ssh into the firewalls to access the firewalls and document the rules, configuration, and networks. There are two firewalls: Firewall A and Firewall B. The main goal for the firewalls is for only one to be working at all times, if one is to fail the other notices that and goes online. That is our goal for this project to automation everything to run on it own. 

For firewall A and B we had SSH into them using the credentials we were provided with. Then install nmap, nmap is a free and open-source security scanner, used to discover hosts and services on a computer network, thus building a "map" of the network. 

To install nmap I ran ```sudo apt-get install nmap```.

Then I ran ```nmap -Pn -A  130.166.240.19``` on the firewall to collect the information we needed.

These are the results I got for Firewall A:

```
techlab@fwa:~$ nmap -Pn -A  130.166.240.19

Starting Nmap 7.60 ( https://nmap.org ) at 2018-11-06 19:42 UTC
Nmap scan report for fwa (130.166.240.19)
Host is up (0.00018s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5d:fe:75:cf:d7:f8:a9:6c:a5:f3:dd:5f:78:bd:a2:2a (RSA)
|   256 4e:17:bb:29:33:08:76:57:8a:dd:56:a1:9a:60:6f:19 (ECDSA)
|_  256 4a:7a:2a:d7:82:23:95:ed:64:dd:20:11:71:1f:e5:13 (EdDSA)
53/tcp open  domain
| dns-nsid: 
|_  bind.version: 9.11.3-1ubuntu1.2-Ubuntu
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.58 seconds
techlab@fwa:~$ uname -r
4.15.0-20-generic
techlab@fwa:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.1 LTS
Release:	18.04
Codename:	bionic
```
For Firewall B I got the same info just as they should be similar:

```
Starting Nmap 7.60 ( https://nmap.org ) at 2018-11-06 20:20 UTC
Nmap scan report for fwb (130.166.240.20)
Host is up (0.00046s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 1a:02:2a:7d:4b:9c:c0:a6:38:13:b7:85:7b:7d:2a:e2 (RSA)
|   256 de:2e:a2:dc:92:b8:63:44:80:fa:65:26:e4:15:97:30 (ECDSA)
|_  256 2f:d4:c1:a9:7d:14:69:fd:0e:31:9e:6c:4d:0d:3a:b1 (EdDSA)
53/tcp open  domain
| dns-nsid:
|_  bind.version: 9.11.3-1ubuntu1.2-Ubuntu
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## What's Next?

For next week, me and my team will be using all the information we gathered about the firewalls in order to start on automating the two of them. 
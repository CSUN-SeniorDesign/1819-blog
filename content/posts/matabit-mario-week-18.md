---
title: "Matabit Mario Week 18"
date: 2019-02-08T00:40:58-08:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 18"]
layout: 'posts'
---

# Week 18

This week we continued working on the transition of the new servers. In addition I worked with a new protocol called Secure Copy Protocol (SCP) which securely transfers files over a ssh connection. 

# Secure Copy Protocol (SCP)

Secure Copy Protocol (SCP) is a means of securely transferring computer files between a local host and a remote host or between two remote hosts. It is based on the Secure Shell (SSH) protocol. "SCP" commonly refers to both the Secure Copy Protocol and the program itself.

## Installation

Secure Copy Protocol does not require any installation, it runs directly from a command prompt window.

## How to use SCP

SCP is fairly simple to use, all you must know is the path to the file and folders you desire as well as the ip address to the destination. 

With SCP you can transfer files or even folders containing files. To perform this procedure it is a simple one line command.

For files the command looks as so: 
```
scp /path/file.txt 10.10.10.10:/root/
```

For folders the command changes slightly:
```
scp -r /path/foldername 10.10.10.10:/root/
```

## My results using SCP

For our Advancing Tech Project I used SCP to transfer over configuration files from one server to another server. The reason for doing this is because we want to replace an older server with a newer one we installed recently. We want the new server to have the same configuration as the older one, so the files we had to trasnfer over were the firewall rules, DNS, and DHCP. To securely achieve that we need to use SCP over SSH. 

Here are the results of using SCP from the old server to the new one:

Old Server
```
techlab@fwa:/etc$ scp -r /etc/iptables 130.166.47.211:/home/techlab/Firewall
techlab@130.166.47.211's password: 
rules.v4                                      100%   20KB  13.5MB/s   00:00    
rules.v6                                      100%  192   215.7KB/s   00:00    
techlab@fwa:/etc$ 

```
New Server
```
techlab@firewalla:~$ ls
Firewall  mario.txt  test
techlab@firewalla:~$ cd Firewall
techlab@firewalla:~/Firewall$ ls
iptables  iptables.rules
techlab@firewalla:~/Firewall$ cd iptables/
techlab@firewalla:~/Firewall/iptables$ ls
rules.v4  rules.v6
techlab@firewalla:~/Firewall/iptables$ 
```
The files were transferred over successfully using the command ```scp -r /etc/iptables 130.166.47.211:/home/techlab/Firewall``` for the firewall folder and ```scp iptables.rules 130.166.47.211:/home/techlab/Firewall``` for the iptable.rules file. For now this is a good sign, next up is to repeat these steps with the rest of the conf files and hope for a successful transfer. One thing I do have to lookout for is duplication of files. I have to make sure the conf files are going to be the same once transferred over.
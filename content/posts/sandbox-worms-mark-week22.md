---
title: "Mark's Blog Week 22"
date: 2019-03-08T20:55:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 22]
Author: "Mark Siegmund"
---

Marks Blog Week 22						March 08, 2019

This week was my worst at getting work done.  I have been completely wiped out this week.  I worked on the Ansible project this week and my group started working with DHCP and DNS on the new servers.

On the Ansible project I got 2 systems setup with Ansible 2.6.5. This was a major chore as my systems did not want to do the install.  I tried on 2 different machines and got messages that I dont have space in /var/cache/apt/archives.  I am not 100% sure but it seems to have something to do with the fact that I am using a dynamic disk and the expansion of the drive was not working for writing in this directory.  I finally managed to do an install on a fixed disk of 30Gig and everything worked.  I ended up making 3 virtual machines that had the same problem, except the last one.  Seems like lots of wasted time just to get Ansible installed.

I was able to create a logon to a second machine using a certificate.  I as able to run ansible commands like ls -a that do a remote command.  Where I got stuck was with elevation of privilege.  If I connect up to a remote machine as a regular user that is in the sudo list, then I am just running commands that are non privileged.  How to do a sudo command and not put password in ?  I could not figure it out.  I will talk with my group this week end to find out.

On the new Firewall B I was working with changing the default gateway and making it permanent.  Seems like if I do it with netplan it is permanent.  When using the command

route add default gw 130.166.240.17 enp4sof1

I had mixed results.  One time it was temporary and at a reboot I had to re-enter the command.

Using netplan seemed to make it stay.

DNS is having a problem it can not resolve anything.  Put using an IP works just fine.  I will be working on that over this weekend.
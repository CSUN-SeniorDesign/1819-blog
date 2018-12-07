---
title: "Matabit Mario Week 15"
date: 2018-12-06T21:04:45-08:00
draft: false
layout: 'posts'
tags: ["Mario Kassab", "Matabit", "Week 15"]
---

# Week 15

In the beginning of week 15, we were informed by professor Weigley that he is getting two new servers to replace firewalls A and B. Our task is to configure the two servers and setup them up to replace the existing firewalls. 
I met up with two other team members and we took a look at one of the servers to see what we are working with and find out if there are any issues (hardware wise). We also had to install an OS onto it, configure the drives in a raid, and confirm it can connect to a network and allow to ssh into it. 

The first task was installing Ubuntu Lts onto the server which is fairly simple. All we had to do was make a boot-able ISO image of Ubuntu on a flash drive and connect it to the server. Once the servers reads the OS we simply follow the install guide of Ubuntu. 

The task after installing Ubuntu was to setup the three hard drives installed into the server in a raid 10. Fairly simple, all you have to do is choose two of the drives and set them to mirror each other. The last drive is set as a spare in case one of the drives fails, it replaces it temporarily until the drive is replaced. 

The last task we worked on was connecting the server to a network and ssh into the server from another PC. Once we connected a Ethernet cable into the server it automatically got assigned a IP address. We ran ```ifconfig``` in the console to view the IP address. We took the IP address and used putty to ssh into the server, confirming the server is up and running and allowing us to ssh into it. 


 
---
title: "Sandbox Worms Yerden Week14"
date: 2018-12-01T14:48:35-08:00
draft: false
Categories: [Advanced Tech]
Tags: ["Yerden Zhursinbek", "Sandbox Worms", "Week 12"]
---

This week we I have been focusing on failover services, I had to make some research on PXE server which will help us configure and boot network computers that are not loaded with operating system yet. It will also provide Network Bootstrap Program that automates the booting of the operating system and other configuration steps.
Failover script is another failover service aspect I have been working on, it has a basic function of checking the status of a remote server to see if it is up or not, and startup the service that is needed on local machine. Scripts troubleshooting process:
•    Ping remote system    
•    Check if port for service is open  
•    If remote system or port is down for a time period, 3-5 minutes, then start up service on local machine.  
•    Loop 
As semester is coming to the end and next week’s presentations are about what we have learned so far it is time to sum up and conclude this semesters experience.

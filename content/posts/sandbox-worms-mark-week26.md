---
title: "Mark's Blog Week 26"
date: 2019-04-12T00:20:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 26]
Author: "Mark Siegmund"
---

Marks Blog Week 26						               April 12, 2019

This week I pressed on working on Firewall Builder.  The executable is fwbuilder.
First problem I encountered was that it would start up but nothing was clickable.  I discovered that a splash screen came up each time I started up fwbuilder.  If I did not close the splash screen it cause this problem.  That one was easy to fix.  Next fwbuilder needed to be run as root.
So    sudo fwbuilder.  From last week when running on windows a program that supports x-windows applications needs to be used.  Moba Xterm is what I chose.
To run a x-window session from a mac or Ubuntu Mario and Thomas found that 
    ssh -X  techlab@130.166.240.19 fwbuilder
Works of their respective machines.  One has Mac OS10 and the other is running Ubuntu 18.10.
When the application does open up it allows me to select a file in the /ect/firewall/ directory.
Files in this folder that belong to fwbuilder have an extension of .FWB
I opened the only file in this directory opened up a firewall rule set that was full of about 20 chains.  I did not understand how the chains were related.  There was a lot of repeated information.   I did a save as to make a copy of this file so that I could modify the copy but the copy did not open up a firewall rule set.  In fact nothing actually opened.  I could see quite a few references to the floating IP 130.166.240.18 and the original ip of 130.166.240.19.  This is a problem because this server now should only have one IP and that is 130.166.240.21.  The floating IP and the other .19 should be removed.
Since there are so many entries for the .18 and .19 addresses in a bunch of chains I just decided to scrap the old ruleset and make a brand new one.  I figured that setting up a single IP of 130.166.240.21 and setting all packets to forwarded to all 3 other interfaces allowing all traffic both in and out.  This would allow all traffic to be passed.  We would have no firewall security but it would also mean that the firewall was most likely working.  Then we could add a rule to do some blocking just to see if it works.  Blocking ssh traffic would be something that show some traffic was working and with the new rule is not working.
Everything seemed to work.  
The interface was defined, 
packets were forwarded was enabled
Traffic was allowed in and out
All ports were set to any any.

The configuration was saved.  ATTechLab.FWB
The compile worked!
The execute command failed.  The error line in the display say simply fatal error.  
I will be working on this failure to make a generic firewall that will be able to execute.
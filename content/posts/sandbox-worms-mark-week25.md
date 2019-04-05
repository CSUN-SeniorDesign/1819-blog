---
title: "Mark's Blog Week 25"
date: 2019-04-05T00:02:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 25]
Author: "Mark Siegmund"
---

Marks Blog Week 25						               April 05, 2019

Goals for this week included 
  finding network IP problem on FWA.
  Backup scripts for Thomas
  DNS - bind working on firewalla
  Setting up firewall  on firewalla

Holiday on Monday
On Tuesday talked to Dr Wiegley about problem with network.  The problem was that netplan on FWA showed only the new IP 130.166.240.21 in the 240 subnet.  However when we tried to SSH to 240.18 or 19 we sometimes connected to FWA (not firewalla as expected).  In addition it was noted over the last few weeks that when connected to FWA we would just get disconnected for no apparent reason.  In retrospect these are classic symptoms of an IP conflict.  I could not see any way that FWA was using either 240.18 or 240.19.  I was using ifconfig to look at network information.   Dr Wiegley pointed out that by using the command
    ip addr list 
That the results were very different.  Ifconfig had been deprecated and the favored use for interface info was the ip command.
Now it was easy to see that FWA was using all three IPs.  130.166.240.18, 240.19 and 240.21.

It is believed that the likely source for this is from firewall rules.

Now to deal with the firewall on FWA.  We found UFW running on FWA.  UFW is known as Uncomplicated Fire Wall (also Ubuntu Fire Wall).  This was something that Dr Weigley was very down on using.  Claiming that it would fail when we try to use the floating IP.  So we set out to figure out what we should be running on our systems.  

So we looked at firewall builder.  This is a tool (GUI based) that will create a firewall rule set.  What it seems to do is to create an executable file that just runs a bunch of iptable commands.
When done it saves a file to /etc/Firewall  .   The file created looks like a XMLtext file with rules imbedded in it.  Next we need to find out what to do with this file.

So at this point I believe that UFW is just a tool that creates routes on our system that are just really a bunch of iptable commands.  I am pretty sure that fwbuilder does the same type of thing.

I will need to use grep to find info on the IPs 130.166.240.18 and .19.   Im guessing.

Firewalld is also a possibility for providing routing services.  But we did not find this running on any of our servers  We did find UFW running on the servers in spite of what Jeff wanted.  I think the last students to work on the system use it as a quick and dirty way to finish out the last project.  It now has been removed and purged using the commands
Apt-get remove ufw
And
Apt-get purge ufw

I could not find any services that looked like a firewall service!
What I found is a service called    netfilter.
It seems to be associated with networking in general.  As long as this service is running iptable commands are processed.

Found this:
https://help.ubuntu.com/lts/serverguide/firewall.html.en

The Linux kernel includes the Netfilter subsystem, which is used to manipulate or decide the fate of network traffic headed into or through your server. All modern Linux firewall solutions use this system for packet filtering.
The kernel's packet filtering system would be of little use to administrators without a userspace interface to manage it. This is the purpose of iptables: When a packet reaches your server, it will be handed off to the Netfilter subsystem for acceptance, manipulation, or rejection based on the rules supplied to it from userspace via iptables. Thus, iptables is all you need to manage your firewall, if you're familiar with it, but many frontends are available to simplify the task.
 
Netfilter is the key service.  We could try and see what happens if we turn this off…?
 
Using service --status-all         will list all the running services
Systemctl status bind9
Systemctl status netfilter
Systemctl status firewalld
Iptables -L  list all the routes
Iptables -L -n -v    list all routes and chain information
Firewall-cmd --status
 
 
/etc/iptables/rules.v4      file with routing information for rules generate from iptables
/etc/Firewall     directory that contains firewall builder files
 
We did not make progress with bind9!!!!
Thomas is working on improving script to include all server configurations to be backed up to Gitlab.  Currently only firewalla is being backed up.  Thomas is using a different token for each server so that its files can be stored in Gitlab.  From a security point of views it is safer than using the same token for each server…. Not sure that you could even use the same token. Lol
 
The last note is in trying to get fwbuilder to run on a windows machine a X-client application is needed.  Moba XTerm is a great choice.  It has support for running an X-window application remotely on a linux system.  Everything is integrated and works perfectly.
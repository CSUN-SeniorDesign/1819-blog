---
title: "Mark's Blog Week 21"
date: 2019-03-01T21:05:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 21]
Author: "Mark Siegmund"
---

Marks Blog Week 21						March 01, 2019

This week I really pushed to get the new servers working.    This week was all about networking and organizing the transition.  Monday was for the presentations and Wednesday was a class assignment for setting up multiple web sites. So we really only had 2 days to work on the servers together. 
We decided that the steps to follow (this keeps on changing) would be 
Get network interface up and working.
Turn off old Firewall A
Set up DHCP on new Firewall A
Set up DNS on new Firewall A
Setup the new firewall on Firewall A
Do the same for firewall B
Incorporate Thomas’s scripting solution.
Work on the failover services.

Ms. Smith pointed out that virtualization was also something we need to look at.  I noted on the Moodle server that there was a bunch of ISO files for different versions of Linux.  This could be a host for virtual machines.

  
The web server assignment was a good assignment this week.  I took much longer to do the assignment than the others in my group.  In the end I think I found some of the directions were not working for me.  I plan on talking to Ms. Smith on Monday or send her an email about my questions.  Bottom line is that there are 3 curl commands to run that verify multiple sites are up and working.  
Just get the first site up and do the curl.  This worked and I was able to see the web server from my host browser. (I was using virtualbox)  
Create site 2 and set up a vHost.  This worked if I used the loopback on my virtual box host, but it did not work from my host OS (Windows).  Curl site 2.   I was not able to figure this out and went on to the third step
Set up an alias directive - again this worked by using the loopback but not from my host machine.

We started on Tuesday with using netplan on the new interfaces.  Making manual changes on one network interface at a time.  This worked with mixed results.  We found the spacing in the yaml file to be very critical.   If spacing is incorrect the line may be looked at as belonging to another option.  This happened to us and when we tried to apply the new plan we lost connection to the server.  All interfaces were now not reachable by any remote system.  Luckily we were able to attach to the iLo port on the server and restart the server.  Everything began working after the reboot.  This happened server times and easily corrected the problem by rebooting.  Several times we set IPs in netplan that were applied but did not work when we trie to ping them.  
I found that if there are multiple yaml files in /etc/netplan each one if applied in order of the first characters.  The default name began with 50.  The full name was 50-cloud-init.yaml .  We made a copy of this file to start with but the name ended in .bak so it was not used.  We could use 1 file per interface and call the files
0eth.yaml
1eth.yaml
2eth.yaml
Etc
But it is easier to but all interfaces in 1 file.
The default settings for the interfaces in the yaml file are all set to DHCP so we had to override all these settings.
I found out that the networking interfaces on our switch really did not have anything to do with the network that we assigned to each interface.  When we configured our interfaces we set only the ip and netmask….. But no gateway.   This was a real duh moment when I realized that all we needed to do was to assign the correct gateway and then found everything was working.  It took me all this time (about 4 weeks) to figure this out.  I only tested the gateway pinging this afternoon.  I now realize that all traffic on a switch is on all switch ports.  The only difference is the network settings on the machine I am using.  All I can say is DUH again.

Another dumb mistake is that somewhere we started using the defalut gateway as 130.166.47.1.  This was totally wrong and it took some looking to figure out that we should be using 130.166.240.17.

The network is 130.166.240.16
The gateway is 130.166.240.17
The floating IP is 130.166.240.18
130.166.240.19 is Old FWA
130.166.240.20 is Old FWB
130.166.240.21 is New FWA
130.166.240.22 is New FWB
And
130.166.240.23 is the broadcast address.
Since this is a /29 this is the entire subnet.


I found several commands to be helpful.
netstat -nr     This will show the gateway of all interfaces.
ip route          Does the same
ip route add default via  130.166.240.17 dev enp3s0f0
This adds the gateway without using netplan. 
ip link show
ip address show
ip addr show     is shorter
ip link set device up (or down)
Ip addr flush dev enp3s0f0

We needed to go over to the MDF to get the networking info so that I could pass it to one of the IT networking guys in campus IT.  He was very helpful and set up our network ports quickly.


We are now ready to start playing with the services and getting them to work.

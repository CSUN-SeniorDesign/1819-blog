---
title: "Mark's Blog Week 27"
date: 2019-04-19T19:20:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 27]
Author: "Mark Siegmund"
---

Marks Blog Week 27						               April 19, 2019

This week made progress on firewall. 
When I looked at the iptable information for the current settings it was very difficult to follow.  It was so daunting that  we struggled with the idea that firewall builder was so complex that it was just easier to use iptable commands to do what we wanted.  I talked with Jeff Wieggley on Tuesday and found that he really wanted us to use firewall builder.  I guess this does make sense when someone has a few notes to understand how to use it, its not that bad to follow.  In using the program and setting up the cluster it actually does save a lot of research and testing time with using iptable commands. 


Lots of practice with firewall builder.  First major hurdle was to understand what compile did in comparison to install.  Turned out that the compile builds the iptables commands needed to implement the firewall.  By choosing the iptable option the file generated had all the commands to set up the firewall.  The file was an executable script.  The next thing that I figured out was that the install option could do the compile but its main feature is to transfer the compiled files to another host or computer using scp.  This means that a host, username and password need to be set up to make this work.  So I used the compile command and I used the file locally…. No install needed.

After using iptables commands it is most important to understand that all system changes will be lost if there is a reboot!!!   By using netfilter-persistent (start, stop, save, and flush) save this will the configuration so that a reboot will not lose the settings.  This needed to be installed on firewalla and firewallb.

Jeff also told me that we needed to use conntrackd.  This is a service that keeps track of all connections that are active.  If the server goes down this service will allow active connections to be transferred to a different machine in the cluster.  If all works correctly your connection will not be lost if the server you are ssh’ed to dies. You will just be connected to the floating server.

We now hahad to set up a service called keepalived.  By setting a priority in the config 2 or more systems would be able to transfer the active ip from one system to another.  In our case we are using 130.166.240.18 as our floating ip.  We set the priority to 100 on firewalla and on firewallb we set the priority to 95.  This means that firewalla is the preferred server and the ip of 240.18 shows active on firewalla.  When firewalla is turned off then firewallb immediately shows the 130.166.240.18 start working on it.  A secondary test for the transfer is to just change the priority in one of the servers so that it mover on the other side of the setting on the other server.  Sounds confusing but if the 2 server are set to 100 and 95 just change the 95 to 105 and that server immediately takes over the floating ip.  The floating ip needs to be changed in net plan to
Read this way for firewalla  [130.166.240.19/29, 130.166.240.18/32]
And on firewallb   [130.166.240.20/29, 130.166.240.18/32]
Then do a netplan apply on each.

We found that due to help that Jeff gave me on Tuesday that something is blocking ssh (mostlike all access) to the moodle server and Lorentz’s machine.  I have emailed Jeff about the problem and am waiting on a response from him.  I believe the problem is that by forcing the new machine firewalla to take the 240.19 ip and most important the old FWA that had a working firewall to have its ip changed to 240.21.  
I worked on this problem all day Thursday and had help from Lisa trying to make sure no firewall was blocking the ssh traffic.  But in the end it looked like we got the firewall working to pass all ssh traffic and nothing else but still no connection or ping would work.  

** one more important change that needs to be done to allow forwarding to function is
   /proc/sys/net/ipv4/ip_forward
This file needs to be edited to have a 1 in the file and then save it.
The default setting is 0.  This disallows traffic to be forwarded.

*** I did an iptables -F.  I was connected with ssh and lost my connection to the server.  I was not able to reconnect to the server.  This commands blows away all routing.  Due to the way the routing was configured all traffic was dropped that tries to get to the server.  The only 2 ways to fix the problem is1) to logon to the console in the MDF and do afix from there or 2) cause the system to do a  power restart.  This is what I did and saved a trip to the MDF.   I am looking for a better explaination on this.  I think I am missing some details
---
title: "Mark's Blog Week 24"
date: 2019-03-29T18:15:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 24]
Author: "Mark Siegmund"
---

Marks Blog Week 24						               March 29, 2019

This week is an odd week because it is Spring Break.  Nevertheless, we still did work this week.  I worked with Mario cleaning up the networking configuration on the new Firewall A server.

I showed Mario how to use Netplan to set up a network interface the way I did last week.  So today Mario set up the other 3 network interfaces.  Each interface is setup with a static lease or each IP is hard coded using netplan.  We made sure that we could access each interface using SSH and using ping on the same IPs.  DNS is working only because we used netplan to point to the DNS server.  At this time it was pointing to our Old FWB server.  At this point in time the Old FWA server was turned off.  The New FWA has no services setup and working.
As expected the only IP we could ssh to from outside the network was 130.166.240.19.  All the 3 other interfaces are not on networks that are accessible to external networks.  

This is where we should have been at end of last sprint, with all network interfaces working.  Now that all networking is behaving correctly we are ready for DNS.

First establish that the New FWA does not have a working DNS service.
This was checked with  the command:    sudo service bind9 status
Everything came back working as expected … ie bind is down.
But we were able to ping www.csun.edu because the DNS service was being pointed to by netplan utilizing DNS on Old FWB.

So we now sshed into the Old FWB and verified that DNS was working with a ping www.csun.edu.  It was working.  So now we turned off DNS on Old FWB with the command:
Sudo service bind9 stop.  Next we did a ping of www.csun.edu and as expected DNS had stopped working on Old FWB.  We now went back to New FWA and did a ping test to www.csun.edu.  It was not working now with DNS but is was working if we did a ping direct to the ip 130.166.238.195.  

Before turning on bind9 on the new FWA we need to make sure that all the files copied over to the new FWA location from the Old FWA server.  All the files looked like the origonal files from Old FWA.  

Now is the real test.  On the new FWA server we did:   sudo service bind9 start
The command came back with no errors.  So next we did sudo service bind9 status.  This verified that all is working.  According to netplan DNS is from the loopback or local machine or FWB.  But we stopped the service on old be so any pings to www.csun.edu would be working only if the local DNS was working….

Yay we got it working.

The networking still has not been completed.  In fact the server may be functioning in terms of network traffic but the ports are not organized even a little bit. That was a major goal of mine this week.  We broke it up over two days.  Each day we spent about 3 hours making the changes and testing each step.  We started with firewalla and FWA.  “firewalla” is the new server and FWA is the old server.  The IP needed to be exchanged between the 2 servers.  It was a lot of editing the netplan yaml files (50-cloud-init.yaml) and using netplan to make the changes.  In the process there was checking of services to see if the services were running.  On the old servers we wanted to be sure services were turned off and on the new servers the services were to be turned on.  The services were DHCP, DNS, and route tables.  


For the future we need to not forget these important changes
Floating IP - was causing a connection to the wrong server.  I am wondering if this is due to routing rules on the campus infrastructure.  I have to find out the answer from Will Moran.
When ssh-ing to the floating IP, 130.166.240.18 the connection is always made to FWA even if I have hard coded the address.  I am thinking this may be due to configuration of a layer 3 switch….  Not sure but something odd is happening.  The only way to deal with the config was to turn off firewalla and configure firewallb without a floating ip.  Then turn on firewalla with iLo and  take off the floating IP on firewalla.

This left the machines working but with no floating IP.

When we did the presentation this week DNS failed  on firewalla.  Just did not make sense why this happened.  It was working the week before and I had just tested it before the class presentation and it was working.  After making all the IP changes on all 4 of the servers DNS now seems to be working.

While making all the network changes we got a report from Jeff Wiegley that Professor Lorentz’s machine (IP 130.166.47.16) was not working.  Professor Lorentz was not able to ssh into his server.  As it turns out I learned that there was one special rule that Will had put on campus equipment.  That was that 130.166.47.16 has a campus routing rule that it is sent to the floating IP which is 130.166.240.18 and that instead of going to that IP is ends up at the old FWA which has routing tables turned on and the traffic is forwarded to the internal network of 130.166.47.0/24.

We did the Prometheus Lab this week using Docker machines.  It was great and I learned a lot.  Docker will bring up a Ubuntu machine in a minute or two.  
docker exec -it <container_name> bash  ----allows one to connect up to a 2nd docker machine 


Network info for our 130.166.240.16/29
130.166.240.16   network
130.166.240.17   gateway
130.166.240.18   floating IP
130.166.240.19   firewalla
130.166.240.20   firewallb
130.166.240.21   FWA
130.166.240.22   FWB
130.166.240.23   broadcast ip

Last thing is that Jeff told us today that we should not be using UFW for a firewall service.
He want us to remove it and be sure that IP tables are working and we are using firewall builder.




To do list 
     Floating ip
     DNS errors that continue to pop up indicating a permission error with the permission 
            on the zone files.
      Test DHCP
     Remove UFW completely (per Jeff)
     Firmware upgrade on FWB (the old server) because we can not ssh to the iLo port
     Get rid of UFB (firewall service)
     Find out why floating IP is not working - call to Will Moran

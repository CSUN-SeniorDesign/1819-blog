---
title: "Mark's Blog Week 29"
date: 2019-05-03T17:35:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 29]
Author: "Mark Siegmund"
---

Marks Blog Week 29						               May 03, 2019

Intense and crazy busy week!
Corosync - communication between cluster nodes
Pacemaker - monitors cluster members, STONITH, CRM
Crm sh -  a piece of Pacemaker…. Needs to be installed separately
Heartbeat -
Pcs - clustermgt ---front end for Corosync - can change corosync settings
Fence-agents-all - uses power managment using iLo ports

Dhcp

Firewall
   Updates
    Ssh - lorentz
    Corosync port

Getting close to the end… one more week.  So much to do
I am working on DHCP this week.  I have the /etc/dhcp/dhcpd.conf re-configured for the new servers.  I cleaned up the old entries.  Set correct ip on old server to the new ones.  This is not super important for actual working DHCP because the servers are all hard coded or have “static” IPs.
I had to make a new netplan so we can test the failover for the frontend network.  This is Jeff’s label for the 130.166.47.0/24 network.  Just to keep things straight….
Up-link is 130.166.240.0/29
Frontend is 130.166.47.0/24
Backend is 10.47.0.0/16
Heartbeat is 10.0.47.0/24

I have setup the the DHCP server to hand out an IP to one of the old servers FWB.  This is just a test to show that the server is actually working.

I found that I could ping anything on the 47 and ssh to anything on the 47.  This is a firewall problem because only 3 hosts should be able for anyone to ssh in from outside the firewall.
The host that anyone can get to from outside the techlab firewall are:
Moodle 130.166.47.10    Hostname is Moodle
Lorentz  130.166.47.16  Hostname is hph9a
NEOS machines - a project that Jeff set up - IP range 130.166.47.100 to 129



Another firewall issue that needs to be resolved is updates.  Doing an sudo apt-get upgrade gives this error:

E: Failed to fetch http://archive.ubuntu.com/ubuntu/pool/main/o/openldap/libldap-common_2.4.46+dfsg-5ubuntu1.2_all.deb  Cannot initiate the connection to archive.ubuntu.com:80 (2001:67c:1360:8001::17). - connect (101: Network is unreachable) Cannot initiate the connection to archive.ubuntu.com:80 (2001:67c:1560:8001::11). - connect (101: Network is unreachable) Cannot initiate the connection to archive.ubuntu.com:80 (2001:67c:1560:8001::14). - connect (101: Network is unreachable) Cannot initiate the connection to archive.ubuntu.com:80 (2001:67c:1360:8001::21). - connect (101: Network is unreachable)



I also worked on DNS today.  We are getting an error : 


? bind9.service - BIND Domain Name Server
   Loaded: loaded (/lib/systemd/system/bind9.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2019-05-02 06:21:54 PDT; 14h ago
     Docs: man:named(8)
 Main PID: 24721 (named)
    Tasks: 27 (limit: 4915)
   Memory: 117.1M
   CGroup: /system.slice/bind9.service
           ??24721 /usr/sbin/named -u bind

May 02 20:24:53 firewalla named[24721]: client @0x7f383c76c440 10.47.0.2#32859/key dhcpupdate: updating zone 'techlab.csun.edu/IN': update uns
May 02 20:24:53 firewalla named[24721]: client @0x7f383d5ae0f0 10.47.0.2#58287/key dhcpupdate: updating zone 'techlab.csun.edu/IN': update uns
May 02 20:28:24 firewalla named[24721]: dumping master file: /etc/bind/zones/tmp-hPUYXwLVEQ: open: permission denied
May 02 20:32:59 firewalla named[24721]: client @0x7f383c76c440 10.47.0.2#45717/key dhcpupdate: updating zone 'techlab.csun.edu/IN': update uns
May 02 20:32:59 firewalla named[24721]: client @0x7f383d5ae0f0 10.47.0.2#38919/key dhcpupdate: signer "dhcpupdate" approved
May 02 20:32:59 firewalla named[24721]: client @0x7f383d5ae0f0 10.47.0.2#38919/key dhcpupdate: updating zone 'techlab.csun.edu/IN': deleting r
May 02 20:32:59 firewalla named[24721]: client @0x7f383d5ae0f0 10.47.0.2#38919/key dhcpupdate: updating zone 'techlab.csun.edu/IN': adding an
May 02 20:32:59 firewalla named[24721]: client @0x7f383c76c440 10.47.0.2#57151/key dhcpupdate: signer "dhcpupdate" approved
May 02 20:32:59 firewalla named[24721]: client @0x7f383c76c440 10.47.0.2#57151/key dhcpupdate: updating zone '47.166.130.in-addr.arpa/IN': del
May 02 20:32:59 firewalla named[24721]: client @0x7f383c76c440 10.47.0.2#57151/key dhcpupdate: updating zone '47.166.130.in-addr.arpa/IN': add

I tried changing group ownership from bind to root on the /etc/bind/zone and all files within that dir.  I find the errors by doing this at the user  ($) prompt 
systemctl status bind9
I makes no difference if you run it with sudo or from the standard user prompt.  

From the last sprint we had decided to split the work this way.  
Mark finish and get working DHCP and DNS… enough to show a little demo that they are working.

Thomas - Is doing testing and documentation on the Firewall and have a little demo on how to use it for something simple.

John and Mario will be working on Corosync and Pacemaker.  I am thinking that they will split them up and do a demo for each service.  What we found is that it got a bit harder than than.





Corosync is an open source program that provides cluster membership and messaging capabilities, often referred to as the messaging layer, to client servers.

Pacemaker is an open source cluster resource manager (CRM), a system that coordinates resources and services that are managed and made highly available by a cluster. In essence, Corosync enables servers to communicate as a cluster, while Pacemaker provides the ability to control how the cluster behaves.


Crmsh is a utility to allows us to set the floating IP up so that it floats.  This utility sends a command that set up the floating ability.  Kudos to John who found this command and it seems to work pretty well.  It looks like we need pcs which is a utility that appears to be a front end for both Corosync and Pacemaker.  Mario installed PCS today but things were not working as expected.

We still need to install the heartbeat package too.  
I have run into another package we may need ant that is:  fence-agents-all
This has to do with the STONITH

--------------------------
STONITH (Shoot The Other Node In The Head)
Stonith is our fencing implementation. It provides the node level fencing.

The stonith and fencing terms are often used interchangeably here as well as in other texts.

The stonith subsystem consists of two components:

           pacemaker-fenced

           stonith plugins

pacemaker-fenced
pacemaker-fenced is a daemon which may be accessed by the local processes or over the network. It accepts commands which correspond to fencing operations: reset, power-off, and power-on. It may also check the status of the fencing device.

pacemaker-fenced runs on every node in the CRM HA cluster. The pacemaker-fenced instance running on the DC node receives a fencing request from the CRM. It is up to this and other pacemaker-fenced programs to carry out the desired fencing operation.

Stonith plugins
For every supported fencing device there is a stonith plugin which is capable of controlling that device. A stonith plugin is the interface to the fencing device. All stonith plugins look the same to pacemaker-fenced, but are quite different on the other side reflecting the nature of the fencing device.

Some plugins support more than one device. A typical example is ipmilan (or external/ipmi) which implements the IPMI protocol and can control any device which supports this protocol.

CRM stonith configuration
The fencing configuration consists of one or more stonith resources.

A stonith resource is a resource of class stonith and it is configured just like any other resource. The list of parameters (attributes) depend on and are specific to a stonith type. Use the stonith(1) program to see the list:

$ stonith -t ibmhmc -n
ipaddr
$ stonith -t ipmilan -n
hostname  ipaddr  port  auth  priv  login  password reset_method

It is easy to guess the class of a fencing device from the set of attribute names.

A short help text is also available:

$ stonith -t ibmhmc -h
STONITH Device: ibmhmc - IBM Hardware Management Console (HMC)
Use for IBM i5, p5, pSeries and OpenPower systems managed by HMC
  Optional parameter name managedsyspat is white-space delimited
list of patterns used to match managed system names; if last
character is '*', all names that begin with the pattern are matched
  Optional parameter name password is password for hscroot if
passwordless ssh access to HMC has NOT been setup (to do so,
it is necessary to create a public/private key pair with
empty passphrase - see "Configure the OpenSSH client" in the
redbook for more details)
For more information see
http://publib-b.boulder.ibm.com/redbooks.nsf/RedbookAbstracts/SG247038.html


Seems to be getting complicated.  I have serious doubt that we will get this working the way jeff wants….
https://medium.com/@yenthanh/high-availability-using-corosync-pacemaker-on-ubuntu-16-04-bdebc6183fc5
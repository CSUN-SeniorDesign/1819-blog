---
title: "Matabit Mario Week 17"
date: 2019-02-01T19:10:28-08:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 17"]
layout: 'posts'
---

# Week 2

This week mostly consisted of working on hardware and setting up the two new servers for installation. Before we went and did that my team and I looked further into the firewall and all the rules contained in the server. We had to understand each rule one by one to find out what they do exactly so we can move along and implement the firewall into the two new servers we are installing so we can automate it.

### Hardware Setup 

Working on the hardware consisted of us setting up the two new servers which included installing an OS onto them, setting up the raid configuration, setting up ilo for diagnosis, and finally placing them on the racks. The first thing we had to was to replace the motherboard on one of the servers because it had a defective Ethernet port. After that was completed we began with the installation of the OS, which we went with Ubuntu server 18.10. At first we went with Ubuntu 18.04 LTS because it had better support for both servers and then later found out that one of the servers Ethernet port 0 was not working on it. So we decided to just run with the latest Ubuntu version of 18.10 which fixed the issue with the Ethernet port. After finishing the installation of the OS we had to configure the drives on the servers to a raid 1+0, which mirrors two of the drives and uses one as a backup in case one of the drives fails it will take its spot. The next step was to setup ILO login on both servers according to the sticker documentation provided physically on the servers. The last step was to physically put both servers on the racks in the MDF which we accomplished with no issues. After racking both servers we connected them to a monitor and booted them up to make sure they are up and running and also able to ping other networks so we can ssh into them and begin working on them.

### Issues
Several issues we came across this was first the Ethernet port on one of the servers which we were able to fix. The next issue we had was complications with the team and our schedules, the professor wanted to split us in half because not much was getting done. After a team meeting in class we come to an agreement to put more effort into the project and start making progress.

### Whats Next?

For next week, we want start off the week by automating the firewall so we can begin on the DNS/DHCP. First we have to meet with professor Weigley so we can get the correct IP address for our servers, then go ahead and copy over the current firewall rules we have and automate it. 

---
title: "Mark's Blog Week 23"
date: 2019-03-15T01:00:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 23]
Author: "Mark Siegmund"
---

Marks Blog Week 23						March 15, 2019

This week is a real changer…..  (even though I have been sick and not able to go to class).
This week we had no class project that was pressing us.  
The things in front of us are

Finish the networking migration- result will be that New FWA will look like the Old FWA.  
      Networking will all function 
      All IPs will be able to accept ssh connections

     **** iLo on Old B willl not work
                     …. Look at upgrading firmware

Turn services for DSN on first (no dependencies)
      Test by ping ip and FQDN internal(moodle.techlab.csun.edu?) and external(www.csun.edu)
Turn on DHCP next -  
       Test with all know mac addresses

Firewall can go at the same time as DHCP or after

We need to test floating ip between .18 and .19

Problems encountered
Today was a big time suck.   We thought we broke the networking on the new A….. but all it turned out to be was

Today was a really good day.  Everyone was working together.







The basic plan was to 
Have all the important files copied over from old A to new A.  I relied on all the others for information before we turned off Old FWA.
We then turned off Old A
Run NetPlan to set the networking to mimic Old A
Set Host name to “fwa”
Verify networking was in place and working
Turn on DNS (bind) and see if we can get the service up and performing.


Of course things did not go acording to plan.  I had the guys working in the lab together and I was working from home.  I asked them to to split the two changes.  John did the Host name change.  Mario and Thomas did the network changes.  They were supposed to watch each other and let me know when the changes were done so I could look at them.  Surprise…..I got no warning and Mario says its broke.   LOL I guess he was not kidding.  Of course all connections were lost.  No one was able to ssh back in to the 130.166.240.19   Which is the access point for Firewall A.  Error messages were about ssh keys being incorrect or possibly a man in the middle attack.   We were into it about 2.5 hours of preparation and everyone needed to go inside of ½ hour.   So we decided to have the 3 of thim go over to the MDF and logon to the console and make the changes back.   This actually got done and the server was restored back to working condition.   
Everyone worked well together.

I spent the next several hours figuring out that the only mistake most likely was just that… ssh keys that were broken from the IP changes.   All that needed to be done is to delete the keys.
In a Ubuntu system that is in the profile that is being used for connection.  In our case that is the 
user:  techlab
In the techlab home dir there is a .ssh folder and inside that known_hosts
/home/techlab/.ssh/known_hosts

Either we could delete that file or
Run the command       ssh-keygen -R <hostname>  IP or FQDN
On Windows using putty it is in the registry     
        HKEY_CURRENT_USER\Software\SimonTatham


FYI changes that need to happen
1) Hostname mod
Sudo hostnamectl set-hostname *chooseNewName*
Edit    /etc/cloud/cloud.cfg
       Find and change line that say      preserve_hostname: false
       Change it to true
Log off machine
Logback on 
Change should be fixed now.

2)  Netplan
          Edit    /etc/netplan/50-cloud-init.yaml    Changes to all the network interfaces 
         Sudo service networking restart    - so we dont have to actually restart the system

I made sure that I was correct and was able to 
       run netplan apply
       Change host name
       Restart the services
       Test by ssh to the new interface.   It all worked
I will let the guys make the changes to the other 3 interfaces as I only did one for testing purposes.

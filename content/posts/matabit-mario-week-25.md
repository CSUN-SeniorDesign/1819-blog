---
title: "Matabit Mario Week 25"
date: 2019-04-09T11:51:26-07:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 25"]
layout: 'posts'
---
# Week 25 

This week started out with an issue we previously on the servers. The ip address ending in .19 which is new firewall A gave me an error saying broke pipe and kicked me off. When I tried to ssh back into the server, I had to regenerate the ssh key using the command ```ssh-keygen -f “/home/Mario/.ssh/known-hosts” -R “130.166.240.19”```. After regenerating the key, I logged back into the server with the same ip address, however this time it took me to the old firewall A (fwa). I went through all relatable files in old and new firewall A to find out why this is happening, but I still can’t understand why. I randomly get kicked off fwa and must regenerate a key again. But this time when I log back onto the server it takes me to new firewall A which is what we want to happen… After looking around and asking professor weigley questions we have come to realize it might have to do with some rules in the iptables file. We are currently still trying to figure that issue out. 

While looking into the firewall and firewall rules in the iptables we come across a service called ufw which stands for uncomplicated firewall. We heard from professor weigley this is not a good firewall service to use and should be deleted on all the servers. So I went on all 4 servers and managed to delete the ufw service using the commands ```sudo apt-get remove ufw``` and ```sudo apt-get purge ufw```. 
Another thing we looked into was ifconfig, however after a little research we come to find that we shouldn’t be using ifconfig and using ```ip addr list``` instead which shows us more information about the network on the server. After running that code on the old firewall a, we found out the server has all three ip address’s assigned to it, the floating ip .18, new firewall A .19, and old firewall A .21. We come to realize that this is the reason we are having issues with ssh-ing into the servers and always having to generate ssh keys. 

Little mistake I ran into. I was on fwa atleast I thought I was and we wanted to restart the server for testing purposes. When I went to run the command ```shutdown -r now```, it restarted my host machine. I am mentioning this here because I think it was an important thing to learn and look out for. This time I got lucky because all I did was restart the machine, if I was needing to delete a file or service and did not realize I was on the server I could have possibly screwed up my host machine. So now I have a good habit of making sure I am on the server I want to be on before running commands. 

The final task I needed to do was to disable all the services (DHCP, DNS, Firewall) on the old firewall A and firewall B so we would not have any interference with the new firewalls. This way we can also get the new firewalls up and running independently without the need of the old firewalls and so that we are sure the services are coming from the new firewalls only. 

The commands I used for this were:

DNS
```sudo service bind9 stop```
```sudo service bind9 disable```

DHCP
```sudo systemctl stop isc-dhcp-server```
```sudo systemctl disable isc-dhcp-server```

Firewall
```sudo systemctl stop firewalld```
```sudo systemctl diable firewalld```

---
title: "Matabit Mario Week 27"
date: 2019-04-19T19:23:04-07:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 27"]
layout: 'posts'
---

# Week 27

This week we are still dealing with the floating ip issue with our servers. According to the team the floating IP issue has been solved, were not seeing any conflicting IP’s. The reason to how this got solved is unknown, it could have been professor Weigley or something on the machines has changed. For now we let it go and decided to make changes to the netplan config.yaml files on both the Firewall A and Firewall B servers. We added the floating IP address (130.166.240.18) to both firewalls, so now once we ssh into ```130.166.240.18``` it logs us into the Firewall A server. One concern we do have is if Firewall A goes down, does the .18 IP address direct us to Firewall B? And if it does direct us there, where in the servers is this specified. We need to test this, I am afraid if we turn off Firewall A it could cause a conflict because there is no script tell the .18 IP to shift over to Firewall B. So as of now we are in the process of researching and testing out this scenario. 

After some testing, the issue we were afraid of happened. The .18 IP address took us to Firewall B even though Firewall A was still up and running. It was back to stage 1 with the floating IP address. We found a service to install on the servers that would solve this issue, it is called keepalived. 

### Keepalived

Keepalived is used for IP fail-over between two servers. Its facilities for load balancing and high-availability to Linux-based infrastructures. It worked on VRRP (Virtual Router Redundancy Protocol) protocol. In this tutorial, we have configured IP fail-over between two Linux systems running as a load balancer for load balancing and high-availability infrastructures. 

We first installed the required packages to configure keepalivd on the servers using the command 
```sudo apt-get install linux-headers-$(uname -r)```

Next, we installed the service using the command ```sudo apt-get install keepalived```

The third step is to setup the service on the server by editing the keepalived.conf file on both of the servers. 

The Keepalived.conf looks as so:

```
! Configuration File for keepalived

#global_defs {
#   notification_email {
#     sysadmin@mydomain.com
#     support@mydomain.com
#   }
#   notification_email_from lb1@mydomain.com
#   smtp_server localhost
#   smtp_connect_timeout 30
#}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 101
    priority 101 (100 for firewall B)
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.10.121
    }
}
```

The last step is to start the service and test it out. To start the service we simply ran the command ``` sudo service keepalived start```. To test this all we had to do was change the priority number in one of the servers to give it priority. Once that has been changed all we do is log out of Firewall A and ssh into .18 and it automatically directs us to Firewall B which is what we want. So that solves our issue with the floating IP address. 


---
title: "Matabit Mario Week 20"
date: 2019-02-21T21:58:07-08:00
draft: false
tags: ["Mario Kassab ", "Matabit", "Week 20"]
layout: 'posts'
---

# Week 20 
This weeks sprint goals for the team were to create backup and versioning scripts and upload them to Gitlab. Deploy the new DHCP/DNS servers, extend our documentation, and finish the transition to the new servers from the old ones. 

I worked mainly on transferring over the Bind and DHCP/DNS config files from our user directory to the etc directory. After completing the transfer of the config files, we need to restart the services so the keys and files regenerate according to the new servers. Before restarting the services I had to check what services were running and what was not running. 

First command I ran was Nmap after installing it to the server. Nmap helps to discover whether DHCP is currently running and what is the status of it. 

Nmap results:
```
Starting Nmap 7.60 ( https://nmap.org ) at 2019-02-18 20:18 UTC
Pre-scan script results:
| broadcast-dhcp-discover: 
|   Response 1 of 1: 
|     IP Offered: 130.166.47.217
|     DHCP Message Type: DHCPOFFER
|     Server Identifier: 130.166.47.2
|     IP Address Lease Time: 5m00s
|     Subnet Mask: 255.255.255.0
|     Router: 130.166.47.1
|     Domain Name Server: 130.166.47.2, 130.166.47.3
|_    Domain Name: techlab.csun.edu
WARNING: No targets were specified, so 0 hosts scanned.
Nmap done: 0 IP addresses (0 hosts up) scanned in 1.76 seconds
```

The next command I ran was ```service --status-all``` to see all the services running. 

The results showed us that DNS/Bind9 along with many other services are currently running.
```
root@firewalla:/home/techlab# service --status-all
 [ - ]  acpid
 [ + ]  apparmor
 [ + ]  apport
 [ + ]  atd
 [ + ]  bind9
 [ - ]  console-setup.sh
 [ + ]  cron
 [ - ]  cryptdisks
 [ - ]  cryptdisks-early
 [ + ]  dbus
 [ + ]  grub-common
 [ - ]  hwclock.sh
 [ + ]  irqbalance
 [ + ]  iscsid
 [ - ]  keyboard-setup.sh
 [ + ]  kmod
 [ - ]  lvm2
 [ + ]  lvm2-lvmetad
 [ + ]  lvm2-lvmpolld
 [ - ]  mdadm
 [ - ]  mdadm-waitidle
 [ - ]  open-iscsi
 [ - ]  open-vm-tools
 [ - ]  plymouth
 [ - ]  plymouth-log
 [ - ]  procps
 [ - ]  rsync
 [ + ]  rsyslog
 [ - ]  screen-cleanup
 [ + ]  ssh
 [ + ]  udev
 [ + ]  ufw
 [ + ]  unattended-upgrades
 [ - ]  uuidd

```
The next command was specifically for DHCP
```service isc-dhcp-server status```

Results:
```
isc-dhcp-server.service - ISC DHCP IPv4 server
   Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-02-20 22:14:42 UTC; 21h ago
     Docs: man:dhcpd(8)
 Main PID: 1232 (dhcpd)
    Tasks: 1 (limit: 4915)
   Memory: 15.4M
   CGroup: /system.slice/isc-dhcp-server.service
           └─1232 dhcpd -user dhcpd -group dhcpd -f -4 -pf /run/dhcp-server/dhcpd.pid -cf /etc/dhcp/dhcpd.conf

```

The next step was to modify the config files to work with our new servers. Machine IP must be statically assigned. "Hardware ethernet" must be changed according to the new server's MAC address. An additional block may be commented out with IP and MAC information of the previous server. In the case the new/current servers are down, comment out to rollback to the old firewalls; be sure to comment out the other block if done.

Example of the file:
```
host fwa-uplink {
		hardware ethernet 00:15:17:7d:b7:07; 	#Change MAC, use ifconfig
		fixed-address 130.166.240.19;
	}
	
	#host fwa-uplink {
	#	hardware ethernet 00:xx:xx:xx:xx:xx; 	#Change MAC, use ifconfig
	#	fixed-address 130.166.240.19;
	#}

```

### What's Next?
For the next sprint we hope to have the server migration completed. Further our documentation and complete the certificates on the moodle server. 

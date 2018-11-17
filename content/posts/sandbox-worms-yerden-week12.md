---
title: "Sandbox Worms Yerden Week12"
date: 2018-11-16T23:42:38-08:00
draft: false
Categories: [Advanced Tech]
Tags: ["Yerden Zhursinbek", "Sandbox Worms", "Week 12"]
---
Due to the holiday on Monday, and presentations on Thursday so we had just a few days to get any forward with the project. In addition, the main machine along with Firewall A seemed to go down. According to assumptions of one of my teammates this might happened due to power issues related with wildfires. 
Since we were not able to access the files, it was suggested to practice DHCP configuration locally on our own devices. We decided to use Ubuntu on VMs for practicing. After the installation of latest version of ubuntu, I ran this command to install the dhcpd “sudo apt install isc-dhcp-server”.
The path to the main dhcp configuration file is etc/dhcp/dhcpd.conf. There I have added some basic configurations as setting up subnet range, default lease time, max lease time, option domain name and servers. 
After the config file has been changed, I had to restart the dhcpd to apply changes by running “sudo systemctl restart isc-dhcp-server.service”.


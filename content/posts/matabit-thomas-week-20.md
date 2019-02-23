---
title: "Matabit Thomas Week 20"
date: 2019-02-22T22:04:33-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 20"]
draft: false
---

# Week 20
For week 20 I continued development on the bash scripts I wrote to create a version history of important configuration files that our server will be using. I've been using a virtualbox Ubuntu virtual machine in order to practice and test the functionality of the scripts. Last week I was able to create a script that will backup files to a temporary directory and another script that will take those temporary backups and create a version history that can be used as a fail-safe if any configuration files become broken through modification. 

However, the script was only backing up files from a single directory. I would need to modify it to allow it to backup multiple directories containing configuration files so we can backup all the files we would need with a single script rather than running a different script for each service's files. It was pretty simple to do this as it only required copying and pasting sections of code and changing file path definitions to find the new configuration files. After some testing using the virtual machine, I was able to successfully backup all configuration files for DNS, DHCP, and Firewall/IPtables using a single script and another script to create a version history of each service. The versioning script will check for modified files each time it is executed and only create a new version if a modification is found. This will happen independently for each service's files. So if a modification is made to DNS files, a new versions will be made for that but not for DHCP or Firewall if nothing was changed.

The last thing I wanted to add to the scripts was a way to save the version history remotely for added security and accessibility in the event that anything happened to the local machines. At the end of my versioning script I added some lines which will push the versions directory to our gitlab repository. So each time the versioning script is executed, it'll push all the versions to the master branch on our gitlab repository.

All that needs to be done from here to make these scripts work on the actual machines is to modify the file path definitions to match the file locations on the actual machines versus where they are on my own virtual machine. I also need to generate SSH keys on the new machines and add them to an account on our repository to allow the machines to automatically push versions to our master branch. Once these two things are complete we should be able to use these scripts to create local and remote backup histories of our machine's configuration files. This while require some testing once we can transfer our files to the new machines and ensure all the services are running properly.
---
title: "Matabit Thomas Week 19"
date: 2019-02-16T01:16:38-08:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 19"]
draft: false
---

# Week 19
For week 19, the advancing tech group's focus was on figuring out which configuration files would be needed to replicate the current DNS, DHCP, and firewall services on the two new firewall machines. In addition, we wanted to also figure out how we could create backups of these files on a regular basis so that if anything goes wrong or if we break configurations through modification, we have backup versions that we can revert to. My task was to create the script we would need to accomplish this.

I chose to write the script in bash and used an Ubuntu 18.04 virtual machine to practice and test the functionality of the script. I first wrote a script that will copy the live version of a file to a backup directory. In addition to copying the file, it will also append a comment to the beginning of the file that gives a timestamp of when the backup was created. Right now it is planned to run this script every 5 minutes to create a temporary backup as we work on the file. I have also included functionality to include successful backups and error messages to a log file.

I wrote a second bash script that will create a backup history and save up to 5 unique versions of the file to another directory. When this script is executed, it will check the contents of the latest backup created by the first script nd directly compare it to the latest version saved in the backup history. If the files are the same, no backup will be created and if the files are different, a new backup version will be created. The command used to compare the files will ignore the very first line since it was used to create a timestamp from when the file was initially copied in the previous script. There will be up to 5 different versions of the file backed up since it's checking for unique versions. As additional versions are created beyond the first 5, the oldest one will be overwritten. I have also included functionality to print successful executions and errors to a log file. Right now the plan is to run this script once a day at midnight. The time intervals of these scripts may be adjusted as we see fit.

Right now these scripts are still only in the virtual machine and aren't in the live servers yet. I still need to discuss with the group which files are the ones we need to create backups of so I can add the proper path names in the scripts. The plan for next week is to finish up these scripts, add them to the live servers, create cron jobs to allow them to run, and make sure all of our services on the server are running properly.
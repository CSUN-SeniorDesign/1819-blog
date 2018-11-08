---
title: "Project 1 Week 2 - Hayden"
date: 2018-09-21T14:59:33-07:00
tags: ["Hayden Wilcox", "Zephyr", "Week 10"]
draft: false
---

> Assigned Task: Continue to work on and finish Ansible playbooks
> Last week's error was resolved.
- The cause was revealed to be an issue with ssh'ing.
- The playbook included a port to access (port 80) which caused some interference. Removed this line
- The hosts file was configured incorrectly. Needed to be rewritten to include a user and changed the IP
- The EC2 instance did not have an elastic IP. I made one and added it to the hosts file as opposed to the previous IP.
> The playbook began to run through without errors, but did not function correctly.
> Intended function of playbook:
- Install Nginx on EC2
- Clone GitHub repo on EC2
- Copy files from github repo public file to var/www/html
> I was instructed by the professor to clone the repo to the localhost, not the EC2
> Changed the location of the destination to be on the local host, but the file no longer seemed to be downloading correctly.
- It was in fact downloading, but it was still creating a directory on the EC2 rather than the localhost.
- After several days of testing, the problem turned out to be improper use of the - hosts: module in ansible.
- Added new hosts lines to properly direct the playbook to download and transfer files.
> Though the playbook now ran without errors, there were a number of aspects that needed improvement.
- The task of installing Nginx was given to Daniel, who wrote it into his playbook. I subsequently removed the task from my own.
- All directory paths were hardcoded, which would cause problems when running it on different instances
- After some difficulties with formatting, I was able to use the command {{lookup('env','HOME)}} when writing the paths so that whoever used the playbook, would have files related to the playbook be installed in the directory set to $HOME so that it could be easily found.
- Rather than copy the files directly, we decided to convert it into a tarball first then unzipped onto the instance.
- I made a script which when run would go into the git repo, delete the current public file then make a new one with hugo, then tarball it.
- I changed the copy: module in the playbook to unarchive: in order to take the new tarball and unzip it into the EC2
- I originally used the notify handlers to delete old versions of files, but that caused issues as they would delete files as soon as they were downloaded. I switched to using the file: module with state: absent or using command: rm -rf to delete files in a proper order.
> Finally, there was an issue with the unzipping process. It would unzip a file named public into html rather than the contents of said file, which prevented the hugo site from loading correctly. This was ultimately resolved by editing the Nginx config file to go into the var/www/html/public, rather than just var/www/html.


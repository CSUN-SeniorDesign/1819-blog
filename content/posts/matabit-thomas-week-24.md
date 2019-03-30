---
title: "Matabit Thomas Week 24"
date: 2019-03-29T23:12:33-07:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 24"]
draft: false
---

# Week 24
Over the spring break I worked on implementing gitlab integration into our backup and versioning scripts. After doing some research I determined that the best way to push files into our gitlab repo from our ubuntu firewall machines would be to create a dedicated advancing tech gitlab account and to generate a personal access token that could be used to clone the repo and authenticate the user each time it attempts to make a new push.

I was able to set up a brand new gmail account for advancing tech under advtech.csun@gmail.com and use that google account to create a new gitlab account. After this I was able to go into the user settings and generate a personal access token that will never expire unless revoked. Using the access token when you clone the repo will allow you to authenticate with gitlab without the need to type your username or password each time. This is ideal for pushing to gitlab using a script running as a cronjob.

After running some additional commands to identify the email and username of the gitlab account in use, I was ready to test the scripts for the first time in the live environment. After running the backup and versioning script manually once each time, I was able to verify that they both worked properly by checking the generated log files and seeing the temporary backup directory created and containing copies of all the config files. When checking at gitlab.com I could also see a new "versions" folder that got automatically pushed into the master branch. Everything with the scripts seemed to be working just as intended so I went ahead and created a new crontab that would run the backup script every 5 minutes and the versioning script every night at midnight with sudo priveleges. 

Mark asked if I had also accounted for the resolv.conf file in /home/techlab/etc as part of our DNS files backup. For our DNS backups I only included files in our /etc/bind directory. I was easily able to modify the scripts to allow for the additional backup of the resolv.conf file. However, when I attempted to log into our new firewall A to pull our updated repo with the updated scripts, it logged me into the old firewall A instead. It seemed that both the .19 and .21 IP addresses were leading me to the same location. Because of this I could not update our scripts and test them out to create a new version that would include the resolv file. Hopefully we can soon determine the issue and resolve it.
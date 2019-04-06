---
title: "Matabit Thomas Week 25"
date: 2019-04-05T23:07:23-07:00
layout: 'posts'
tags: ["Thomas", "Matabit", "Week 25"]
draft: false
---

# Week 25
Earlier this week it was discussed in our group that it would be beneficial to expand our backing up capabilities to cover all of our servers including new and old firewalls A and B. This way if any future students were working on them and changing configurations for next semester's projects, they could have regular backups and version creation in the event that they needed to go back to older configurations. Since I was the one who created the scripts in the first place and understood how they worked, I agreed to carry out this task.

The scripts were configured to run on new firewall A only and thus needed to be changed a bit to be able to run on the other servers as well. I decided that each machine should get their own separate versions directory within the main versions directory that gets pushed into gitlab. In order to do this I had to make some modifications to the file paths within the script to tell them to save to their specific versions directory, not to the main one. In the end there was eight scripts in total, two for each of the servers.

After adding all the scripts into our gitlab repo, I created new personal access tokens in gitlab, one to be used by each machine, and cloned the repos. Once each machine had a copy of all the scripts, I specified which ones should be run on which machine by using crontab and selecting the appropriate ones to be executed. After running each server's scripts manually, I checked gitlab to make sure it worked. After checking I could see each machine created its own versions folder within the main versions folder on gitlab, confirming that the scripts worked exactly as I intended. 

A problem I ran into, however, was cloning the gitlab repo on to old firewall A. For whatever reason, gitlab.com would time out each time on port 443. I have a suspicion that this has to do with the current firewall configuration blocking connection on port 443 and I'll have to find a way to modify the rules to allow such a connection so that the repo can be cloned and version pushes can be made regularly. So as of right now, I only have the scripts up an working for new A, new B, and old B. 
---
title: "Project2p1 Hayden"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Hayden Wilcox", "Zephyr", "Week 5"]
date: 2018-09-28T22:06:47-07:00
draft: false
---

For this project it was decided that I would be team leader. As such, one of my major tasks this week was to assign jobs to the others. Due to last project spilling over into this week's, we were set back a bit. We were working on getting things ready to turn in and present up until Sunday night. As a result, Monday's class time was spent with presentations and we decided not to start until Tuesday.

Come Tuesday we combed through the Project 2 pdf together. I took note of the line about "information silos" or members of our group who held all the knowledge about a certain subject. I wanted to
make an effort to avoid that, so I assigned tasks to my team members that were different than what they had worked on before. I assigned the circleCI task to Tonny and Neel who had previously worked
on Terraform together. I did this because the description of it sounded very similar to what Daniel and I had previously worked on before with Ansible, creating a public file, tarballing it and transferring it to the website, but with a few more steps and different coding. For Daniel I assigned him the AWS auto-scaling groups (ASG) as it was closer to Terraform. In fact, it ended up being terraform on a larger scale as I understood it. I initially planned to help him with this, but he was able to complete the task on his own, so I focused on other things.

I dedicated the task of the Data Dog account to myself. Setting it up turned out to be very easy, with a few interesting moments. I realized that though I had signed up for the GitHub
education dev pack, I never actually confirmed it, which led to some confusion. After that, I signed up to Data Dog through a standard process. I was confused at first how to invite the others, almost asking them to sign up for their own accounts, but I realized it was just a simple matter of finding the right option (in my defense, it wasn't very obvious). After sending them all emails, and their confirmations, the account was finalized.

After the account was finished, I wasn't sure where to proceed, and decided that while the others handled the tasks I gave them, I would do some catching up. On Wednesday, I had Neel give me a quick
run through of how to set up an EC2 using terraform. After his work with the last project, the process went over smoothly, with his direct guidance and reading through his documentation, I was able to set up a working EC2 fairly quickly, with only a few minor errors. I feel that there's still more I need to learn to feel comfortable setting it up on my own, but this can be remedied with some self study later.

Finally this week, I decided to try and install and run the Data Dog Agent on my personal EC2 for testing purposes, so that I would be ready to set up and manage it on our project instances
when the time came. Unfortunately this caused some isssues. Firstly, I had to add some more ports to my terraform file to allow the EC2 to interface with the Data Dog Agent's github repository. The Data Dog website advises using a very long command to install the agent on AWS linux in one go, which is why the extra port was required. Unfortunately, there were other problems. Running the command causes a large torrent of errors, which seem to be connection related at first, but the program still seems to connect and attempt to run, only for it to fail due to a currently unknown reason. Going forward, I will need to look into this error in order to get the Agent working so I can perform my duties for next week.
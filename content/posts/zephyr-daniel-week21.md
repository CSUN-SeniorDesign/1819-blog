---
title: "Irrigation Post - Week 21"
date: 2019-03-01T10:44:11-08:00
draft: false
tags: ["Daniel", "Zephyr", "Week 21"]
---

# Week 21
This week I was tasked with working on a new Docker file and continue working on the CI/CD pipeline. For the Dockerfile I decided to use docker compose and compile multiple Dockerfile's instead of just having one big one. I decided to do this because it would make it more modular and if we decided to change one apsect of the container I can just change the module. I think this will make troubleshooting a bit easier as I would be able to see what isn't working more clearly. So currently I have a Directory structure where I have my root and in this root Directory it contains a LAMP stack. Currently I have a php folder that contains a Dockerfile that builds a php container, Mysql folder that containers a Dockerfile that builds a Mysql container another one for apache2. An issue I ran across was that there may be an issue with the apache docker and php docker failing too communicate and too fix this I changed the php container to include apache2. This is done with the image reference and then tagging it with -apache. 

 After the LAMP stack I have to have node.js and Angular, the current issue is that these two things basically do the same thing so maybe after discussing with my team I'll only need to add one to the docker at least to my understanding.

# Tasks as of Now.
After I finish the Docker I'll need to look into getting the CI/CD pipeline working. We have decided to go away from GitLab and move to CircleCI as most of use are more familiar with that software and GitLab was too difficult for me. Hopefully this change will make the implementation of the CI/CD pipeline a little easier. Since I didn't work on the CI/CD pipeline during the last semester this is still something I have to learn and try to implement for the first time.

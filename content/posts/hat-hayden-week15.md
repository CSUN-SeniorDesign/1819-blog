---
title: "Hat-Hayden-Blog-15"
author: Hayden
date: 2018-12-07T14:35:33-08:00
tags: ["Hayden Wilcox", "HAT", "Week 15"]
draft: false
---

This week I was unable to accomplish much for the project directly, as I realized that the group had surpassed my skill set leaving me unable to assist them. To rectify this, I consulted with my teammates on what I needed to learn to be able to help them. I was directed to learn Docker and Docker-Compose to help setting up an image for the CS dev to team to work on, among other subjects. Therefore, for this week I dedicated myself to catching up on that technology first.

To achieve this, I referenced back to previously written documentation by other teams but found it somewhat confusing to get what we needed done. I then looked up official documentation and was able to work with it much better. I went through Docker first, creating an image with Ubuntu as the base image (FROM ubuntu:16.04), and nginx, php and supervisord (RUN apt-get install -y nginx php7.0-fpm supervisor) installed. I also cleared the apt lists in case there's any conflict (rm -rf /var/lib/apt/lists/*). I then needed to set up a number of config files, setting up directories, log files, and opening up ports among other things. I was then able to run it and check my localhost, which had a simple html file that would say "Hello World! This page has been viewed _ times." The blank would increment every time the page was loaded.

At this point, it came to my attention that Docker-Compose was a different program altogether (I thought when my teammates said "compose" it was Docker term I was unfamiliar with). Once again, utilizing official documentation I familiarized myself with Compose. This Dockerfile utilized Alpine, a much lighter weight Linux base image than Ubuntu, and the image that we will be utilizing for our actual project. This also utilized some python, which is another shortcoming that I must rectify if I want to help in the later stages of the project but did not need to completely understand for the purposes of the testing. Utilizing 2 different "services" (builds), web (which makes the base image and forwards port 5000 to port 5000 on the host machine) and redis (which uses a public Redis image pulled from the Docker Hub registry). Once the proper config files were set up, I was able to alter a file in the Docker-Compose directory and have the changes be sent to the var/www/html and immediately update the site itself for testing.

From this point on, I intend to study CI, Lambda and Python over winter break to be better able to assist my team next semester.

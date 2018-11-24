---
title: "Irrigation Post - Week 13"
date: 2018-11-23T16:28:34-08:00
draft: false
tags: ["Daniel", "Zephyr", "Week 13"]
---
# Week 13
Our task for week 13 was to setup the automation needed for the test environment. We were able to setup LAMP stack manually with the files the software team had given us. The files were the website they had setup up locally which we are now trying to implement automatically and using AWS. I have been working on taking what we have setup up manually and the files from the software group into a Dockerfile we can push to give us a container we can use as a baseline. 

The original Dockerfile I had created only contained an image for a LAMP stack with nothing else, I have changed this container using a different method. The new method will include a different base image such as an ubuntu base image and then installing LAMP and other dependencies through RUN commands on Docker. I think this is more fitting as a member in our group has created scripts that allows for the setup to be easier which I've decided to try running through Docker.

# Task as of now
Continue working on the updated Dockerfile and fix out any issues we run into when trying to setup up the LAMP stack and the website. If we are able to get this done we can move onto setting up the AWS service so we can make the website available to the software team.
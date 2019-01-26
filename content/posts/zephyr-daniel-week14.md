---
title: "Irrigation Prost - Week 14"
date: 2018-11-30T18:21:56-08:00
draft: false
tags: ["Daniel", "Zephyr", "Week 13"]
---
# Week 14
Our focus for project 7 was to solidify the foundation needed for the irrigation team for when they needed our resources. Creating the infrastructure needed to host the website they needed. This was a combination of scripts and Ansible to setup our automation. Docker file for the instances needed to push the website using containers. This would be done in conjunction with Terraform that would setup the AWS infrastructure.

My task was too finish up the Dockerfile so it's able to setup the website onto AWS. This would be a different file than the previous one I had created. This is because the base image of the previous file was one that specifically focused on setting up a LAMP stack. If the group wanted more in the container the it would be difficult to do so. I decided changing the base image to any linux one would be sufficient enough and just installing LAMP through commands in the container. Another approach would be maybe doing a multistage Docker container so we can have the best of both worlds. This might be difficult because the LAMP based image is a difficult to run and might have issues running in a multistage Dockerfile.

# Task as of Now
Continue and finish the Docker file so it's able to setup the site. The files that we were given are a bit hard to implement. I'm having trouble interpreting what needs to go where and how to have it setup so the site is available using Docker. 

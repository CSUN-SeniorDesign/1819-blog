---
title: "Project 1 - Post 1: Daniel"
author: "Daniel Hirahoka"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 3"]
date: 2018-09-13T11:05:26-07:00
draft: false
---
<h3>Daniel Hirahoka:</h3>

		- For Project 1 I was assigned the task to assist with deleting the Route53 information in order to prepare for Project 1. 
		After doing so I was additionally tasked with working and researching Ansible in order to complete the Ansible portion of the Project. 
		So for the next couple of days I looked into installing and setting up virtual machines on my computer in order to practice and see how Ansible would work.
		To begin I started reading the quick start guide that Ansible provides which includes a 13 minute video as well as an installation guide. 
		Which is where I encountered my first issue, in order to correctly do this project I would have to install Ansible within the instance that is created using Terraform.
		Knowing this I needed to at least familiarize myself with Ansible before I can confidently attempt the work on our AWS instance. So I broke down my tasks into 3 parts:
		
			  1. Mimic project 0 and practice with this instance as best as I can.
			  2. Document the progression of Ansible using the practice instance.
			  3. Begin the final version of Ansible on our Organization instance.
			  
		- To begin with my tasks I looked at our documentation from Project 0 and began following it and creating a VPC, subnets, and route tables just like if i was doing Project 0 again. 
		The only difference would be I wouldn’t have an actual domain to work with, this hasn’t really been an issue so far. 
		As I am setting up the Instance I am also reading on how Ansible actually works. I’ve learned about Playbooks and how you can break them down into 3 parts.
		It’s separated by your hosts, who the playbook completes tasks as or who it needs to target. 
		Next they have the tasks where you write what the play book does, it’s essentially the job it needs to do. 
		Finally you have your Handlers which seem to work like tasks but if i understand it correctly only run if things change or if they are triggered in some way.
		After reading about playbooks I began reading about the different modules that Ansible has to offer.
		Ansible has 1000’s of modules available for people to use and if you search with AWS in mind, they tag them differently so you can see which of these modules work with AWS.
		My task of now is to continue with the practice and see which of the modules help with the tasks that need to be done in Project 1.

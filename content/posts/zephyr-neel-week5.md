---
title: "Project 2 Post 1 - Neel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 5"]
date: 2018-09-28T21:25:33-07:00
draft: false
---
		Project 2
			
			- tasks:
				1. Creating an IAM user with PUT and GET Object policy for s3 bucket
				2. CircleCI helping Tonny
				3. Help team members understand terraform and assist in any way
				4. Helping Hayden with DataDog Agent
	
			In the beginning of this project, we were told to do things that we have not done in the past project 
		and let someone else have the opportunities to learn terraform. Since I could not do terraform, 
		I was tasked to help Tonny with CircleCI and create an IAM user on aws with specific policy 
		to do curtain action against s3 bucket. Therefore, I started with creating an 
		IAM user and created a new policy. I used the Visual editor to create a new policy 
		where i selected the service: S3, Actions: Read- GetObject; Write- PutObject, Resources: add ARN for the previous project S3 bucket.
		After that, I started with CircleCI by first trying out the hello world project that they have on their website for getting started with CircleCI. 
		After creating the hello world, I had to do the next task, which was to install git and install Hugo using dpkg, 
		but I ran into an error since the image I was using was Alpine Linux and it did not ran Ubuntu commands. 
		So after looking and researching I came across solution was to build a machine which was Ubuntu. 
		After that, I used the Ubuntu commands to install git and Hugo using .deb file from GitHub and removing it after Hugo is installed. 
		After that, Tonny took over the CircleCI part as I was helping other team members understand terraform since Tonny and I were the only one who had knowledge
		in terraform from Project 1. Therefore, I helped Daniel understand how I created my instances and what information he needed to know to work on the auto 
		scaling groups and launch configuration. As the week went all members were working on their part, but Hayden ran into some problems with DataDog and 
		as I had no other task I started helping Hayden and will try to figure out the DataDog during the weekend.



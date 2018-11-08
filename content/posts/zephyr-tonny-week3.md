---
title: "Project 1 - Post 1: Tonny"
author: "Tonny Wong"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 3"]
date: 2018-09-13T11:05:09-07:00
draft: false
---
<h3> Tonny Wong: </h3>

	Assigned duties for this week:
	
			- Learn IAM Terraform
			- Edit Blog post in Hugo 
			- Configure issues in Hugo
			- Test IAM script for Terraform.
			- Learn Editing GitHub Markdown Files.
			
		Learning to setup IAM user in Terraform:
		
			I have went to Terraform tutorials and guides in order to proceed with creating AWS IAM in Terraform:
				- To create a AWS IAM in Terraform we would need to create a file with the extension of .tf
				- Some configuration exaples are within the Terraform documents which I have used a couple of them such as:
				
						- AWS_IAM_ROLE
						- AWS_IAM_INSTANCE_PROFILE
						- AWS_IAM_ROLE_POLICY
				
				- These scripts are given in the Terraform guide, it is where we can setup automation and policies where you can give certain permissions in each persons role.
							- We do have to create a EC2 instance user with Terraform so that the script would work when running on command.
				
		Hugo Blog Post:
			
			- There was an issue with the Hugo Blog since it was giving dependancy issues in github.
					- I have been using the right command to fix the issue but was in the wrong folder.
					- I have then ran a "npm install" and a "npm audit fix" towards the themes folder where the theme that me and my group selected have downloaded.
							- Reading through the npm audit fix i have seen the multiple outdated packets and then fixed.
					- Learned to format separately from markdown file for the website and also for github.
							- Markdown files can be treated as html formatting for Hugo static blog but does not work for github which will be different formatting to get the same results.
									- github can use html formatting but will not work equally.
									- github can call out images this is an example of what can be put in the markdown file:
											-  ![GitHub Logo](/images/logo.png)
					- Then I have the duty to fix all formats for the group members so that it should be easy to read.
---
title: "Project 2 post 2 - Neel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 6"]
date: 2018-10-04T11:04:23-07:00
draft: false
---

		Tasks: 
				Create a base AMI with Packer 
				Create another s3 bucket 
				Create another Hosted Zone to add staging subdomain
				
		Starting the second week i looked into creating a base AMI using Harshi Corp packer 
		so that when Daniel has his auto configuration done i can just give him the AMI i 
		got from packer.So to create base AMI i first installed packer and added the files
		to my environmental variable.After that i create a file with .json extension since
		packer uses json to create images. First i defined my variables so that i can use aws
		with packer using the access key and secret key i got when i created an IAM user last 
		week.After that i started defining what type of instance i want, region, how i am 
		going to communicate with the instance using the builders. After that i created some 
		provisioners where i am some files to the instance for Datadog logs, uploaded two bash scripts 
		created by Tonny. Next is installing nginx and aws cli and changing the file permissions for the
		two scripts to run using the cronjobs. I also added a script to download Datadog using one 
		command. After that i used cmd to build the image using packer bulid nginx.json. At first i only
		had nginx and datadog installed, but as week passed i had to add more scripts and files 
		given to me by other group members. Also when i was uploading files from windows to ubuntu
		the scripts and files had ^M at the end of each line. So to fix i used this: 
		sed -e "s/\^M//" filename > newfilename. After the packer build was done i passed the AMI to Daniel.
		Another task i had was to create another s3 bucket since we were using the terraform bucket we
		created in last project and professor told us to create another one.After that i started working 
		on creating another hosted zone using terraform. Also one of the thing that was done wrong or didn't
		have enough time/help was that i manully had to ssh into our instance to configure nginx for staging.
		So i went /etc/nginx/site-available/default and added another server that listen on same port 80 but will
		point to /var/www/html/staging/public/ files when people go to www.staging.zephyr90.com. On the other hand,
		if user go to www.zephyr90.com they will see differnt website which is our updated blog.I will still work on
		this just to learn.
		
			
---
title: "Project 1 Post 2 Tonny"
author: "Tonny Wong"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 4"]
date: 2018-09-21T21:43:29-07:00
draft: false
---
<h3> Tonny Wong: </h3>

	Duty this week:
		-	Assist all members to understand markdown file and have the hugo site into their local machines so that they could also update and upload their post by themselves.
		-	Worked on terraform (iam aws, backened, bucket, linux server)
	Duties done this week:
		-	We have worked aim script to create all users necessary for the project:
			o	Creating a group administrator
			o	Creating iam policy to give admin access
			o	Create the users
			o	Add all users into a group 
		-	Terraform backend
			o	Determines how state is loaded and how an operation is executed. 
		-	Terraform bucket 
			o	It is used to store state remotely 
			o	The bucket is used to store the state files for multiple access users, the locking state will lock whenever a user is in the progress this is due to the use of dynamo.
		-	Linux server settings are setup 
			o	It was created to create two linux web server in private subnets
		-	Setting up hugo sites to all team members:
			o	Followed same process as Project 0 only that instead of starting from the beginning:
					We have installed Hugo, Chocolatey, Git for the local host computer.
					•	If the user already has the files make sure to update the file by:
			o	Git pull origin master
					This will update the hugo site so that the files would not corrupt each other when files push to the github.
					We have used these commands to setup the hugo folder:
					•	Git init
					•	Git clone <clone link>
					Then we go to our destined hugo blog and create a post.
					After creating a post we would want to submit it to git hub:
					•	Git branch <name of branch>
					•	Git checkout <name of branch>
					•	Git add . 
					•	Git status
						o	To verify the new updates on the branch
					•	Git commit -m “<message>”
					•	Git push -u origin master

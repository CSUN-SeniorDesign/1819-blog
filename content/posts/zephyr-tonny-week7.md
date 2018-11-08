---
title: "Project 3 Post 1 - Tonny"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 7"]
date: 2018-10-12T18:20:37-07:00
draft: false
---
<h3> Tonny Wong: </h3>

	Duty this week:
	
		-	Setup Docker Toolbox on Windows
		
		-	Help troubleshoot Docker image

	Duties done this week:
		
		-	Learning to use Docker and reaserching forms to use it on a local machine.
		
				- Edited installation so that the Default VM would take a IP that will be given to it.
				
					- This is done before openning Quickstart for Docker toolbox.
					
		-	Errors occured when running Quickstart using "Run as administrator" this is because as I did research. Quickstart is already is setup to
			have certain admin powers and that all the research were meant to setup towards standard users. 
			
		-   Then after testing for errors for installation purpose we then tested out if docker was working correctly.
			
				- This is done by running: Docker run hello-world
						
						- If the image does not exist it will try to find it elsewhere or create one in whih will tell you if the docker toolbox is working approapriately.
		
		- Help test run and troubleshoot docker image created by Daniel 
			
				- Helped Daniel to appropriately setup Hugo and Nginx in the Docker file image.
				
						- Still requires work
				
				- Helped trouble shoot and verify if website is running through localhost by using.
				
						- docker run -d -p 1313:1313 hirahokad/zephyr:production
						- docker ps
							- verify if its running
						- <VM ip>:1313
							- docker-machine ip
								- To see what is the ip of the docker-machine in use.
								
		- Next task for the week will be working on using the docker image through CircleCI.
				
						


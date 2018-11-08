---
title: "Project 2 Part 1 Tonny"
author: "Tonny Wong"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 5"]
date: 2018-09-28T21:43:29-07:00
draft: false
---
<h3> Tonny Wong: </h3>

	Duty this week:
	
		- 	Learn about CircleCI
			-	CircleCI will run codes automatically rather than having the user do it manually.
		-	Help setup Hugo blog with CircleCI

	Duties done this week:
		
		-	Neel has begun the procedure to put CircleCI in github and I have finished it through editing the config.yml
			- This is where the code will be ran through CircleCI and what we have so far.
		-	The process so far of what I have placed in CircleCI and done with CircleCI are:
			- Installing aws cli
				- This is where the command will help with the procedure of moving the tar files into the s3 bucket.
					- sudo pip install awscli
			- Removing old public file and creating new one using hugo and Zipping files to tar:
				- This will go to the current blog files that we are using then removing them. Create new public files with hugo and then it will tar the files in the correspondent name.
			        - cd /home/circleci/project/Project1/zephyr/
					- sudo rm -r public/
					- sudo hugo
					- tar -zcvf $CIRCLE_SHA1.tar.gz public/
			- Then the end procedure is adding the tar file to the s3 bucket by copying.
					- aws s3 cp /home/circleci/project/Project1/zephyr/$CIRCLE_SHA1.tar.gz s3://zephyrbuckets3/circleciuploads/  

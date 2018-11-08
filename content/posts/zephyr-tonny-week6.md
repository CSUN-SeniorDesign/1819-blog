---
title: "Project 2 Post 2 Tonny"
author: "Tonny Wong"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 6"]
date: 2018-10-04T00:42:09-07:00
draft: false
---
<h3> Tonny Wong: </h3>

	Duty this week:
	
		-	Create script that would grab the tar file fro s3 bucket.
				-	The script will be also verifying if the tar file is a new tar file or old one.
				-	After verification it will replace the old html public and replace it with the new one.
		- 	Create cron job and test run with a script to deploy the hugo  site. 

	Duties done this week:
		
		-	First it grabs the tar file and places it into a file on linux using this command: (If the linux file do not exist it will create them)
				- sudo aws s3 cp
			- Then it will create 2 more files if they do not exist, to do the verification process. 
				- mkdir -p
			- Then used ls to check whats inside the folder, Meaning that one should contain the tar file.
				- Then it will convert what output from ls into text.
			- I put them into a variable so that the output of the text can be used in the if statement for verification purposes.
				- The verification should do is to verify the tar file.
				- The program has to be ran twice in order to have something in those files.
				- So when we actually start editing tar it will continously grab the most recent updated tar.
					- If the tar is not new it will remove the tar that was downloaded.
		-	After the verification process was done then the tar file will be unzipped and placed in the html public.
			- Before we do put it into the html public we will remove whatever is inside the html and then place it by running this code:
				- tar -C /var/www/html/ -zxvf <wherever the tar file is placed> 
					- this file will be in alone since the tar file name is randomly generated.
					- so that we could use this address an example is :
						- /home/ubuntu/current/*.tar.gz


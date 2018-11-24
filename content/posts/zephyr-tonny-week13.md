---
title: "Zephyr Tonny Week 13"
date: 2018-11-23T20:58:45-08:00
draft: false
---

Tonny Wong:

	Communicated with the other group:
		
		- Add CS Team to AWS and provide them with educate.
		- Create Price table for future spendings and cost analysis.
	
	Communicated with the CS Team so that they would create their aws accounts and adding them into the organization.
		
		- We have successfully managed to host the website locally and now setting things up in terraform, ansible, and dockerfile.

	Task for these two weeks are:
	
		- Create a price chart or cost analysis of what is possibly going to be used to run the website.
		- Create AWS Terraforms
		
	Current Standing:
	
		- Cost analysis within EC2 t2.micro, ALB, S3 Bucket and optional Cloudflare done to ensure the basic cost of these since it will be necesarry.
				- ALB with the given free tier done without free tier would be $27 dollars monthly. Just having the ALB is 17$
				- Having 3 EC2 instances dependng on whihc type can be very expensive so we decided to try t3.micro whihc is $7.62 monthly each.
				- Currently S3 Bucket stands the least which for 100 GB is $2.18 per month which would be a basic number for storage it can increase later on.
				- The reason why the Cloudflare is optional is due to be able to create multiple accounts and having each individual one do a task and the option of instead doing that for the free version there can be the 20$ per month which you are able to do all in one account. 
		- Currently working on Terraforming the manual process into a automated procedure.
		

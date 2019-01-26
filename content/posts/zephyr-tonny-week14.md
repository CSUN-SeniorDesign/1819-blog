---
title: "Zephyr Tonny Week14"
date: 2018-11-30T14:57:18-08:00
draft: false
---

Tonny Wong:

	Communicated with the other group:
		
		- Add CS Team to AWS and provide them with educate..
	
	Communicated with the CS Team so that they would create their aws accounts and adding them into the organization.
		
		- We have successfully managed to host the website locally and now setting things up in terraform, ansible, and dockerfile.

	Task for these two weeks are:
	
		- Create a price chart or cost analysis of what is possibly going to be used to run the website.
		- Create AWS Terraforms
		
	Current Standing:
	
		- Cost analysis within EC2 t3.micro, ALB, S3 Bucket and optional Cloudflare done to ensure the basic cost of these since it will be necesarry and also Route 53. (Monthly
				- Route 53 : 
					- 1 million queries: $0.90 monthly
				- ALB :
					- ALB setup for free tier (actual total) : $22.33
				- EC2 : 
					- t3.micro : $7.62
					- EBS Volume : $0.60
				- S3 Bucket : 
					- 3: S3 Buckets : $7.77
						- 100 GB
						- 1000 request 
				- Cloudflare :
					- Free but optional Professional version $20 monthly
		- Terraform:
			- Building infrastructure using terraform 
				- Creating the VPC
				- Internet Gateway
		- Currently working on:
			Downloaded Terraform
			Downloaded AWS CLI
			Setup AWS with local machine
			Setup one administrator IAM and obtain secret key
				- Subnets (Private and public)
				- Nat instance
				- Bastion host
				- Terraform 
					- IAM
					- S3
					- EC2
					- Route 53
	
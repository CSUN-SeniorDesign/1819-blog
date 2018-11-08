---
title: "Project 1 Post 2: Neel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 4"]
date: 2018-09-21T17:25:57-07:00
draft: false
---
	Tasks: Creating Nat Instance
		   Creating ALB


	Creating Nat instance:
		- During the second week of the project 1 i started working on creating an Nat instances so that when Tonny bring up the private web servers they can access to internet to do updates and etc.
		- The first thing i did was create a security group that allows port 80 and 443 from the private subnets as inbound. Port 22 for SSH from anywhere. Outbound for 80,443. I created this security group using the guide provided by aws.
		- After creating the Nat security group i create aws_instance resource to create already configured nat instance by aws. Since Nat instance has to traffic both ways i had to disable the source/destination check.
		- After the instance i generated an elastic ip for the nat instance.
	Creating ALB
		- After Tonny had the two private web servers up and running my next task was to create Application Load Balancer.
		- When creating the load balancer i was able to make it work by doing it manually using aws console but was having error with terraform.
		- I asked one of the other team in the class who said that terraform should be exactly like how i did it using aws console, so after that first i created alb security group that has port 80 open for all inbound traffic. All traffic for outbound.
		- After creating the security group i created the Application load balancer that was public facing. Also reading about alb on aws documentation it said that we have to assign our public subnets instead of our private subnets.
		- After creating the alb i created a target group that would listen on port 80 for now with health check configuration i got from aws console.
		- I added a listener on port that would forward any traffic coming on port 80 to the target group but later it would redirect to port 443.
		- Then i attached the private instances to the alb target group using port 80.


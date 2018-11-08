---
title: "Project 3 Post 1 - Neel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 7"]
date: 2018-10-12T18:47:16-07:00
draft: false
---

		For the third project my tasks were to work on CircleCI with Tonny after Tonny/Daniel were done with docker image,
		making sure that we have Application Load Balancer and Route53 configured for our cluster. Since Tonny and Daniel were
		working on the Docker image i began helping Daniel by creating AWS Elastic Container Registry using terraform to help Daniel
		out with some of his other tasks. We will be using this repository to store our docker images with tag. We might also need to work
		with permission on the ECR when we do the CircleCI and pushing the docker image to ECR, but for now i just created empty one.
		
			resource "aws_ecr_repository" "zephyrecr" {
				name = "zephyrecr"
			}
		Since we had Application Load Balancer done in other project as well as Route 53 i decided to help
		Hayden out with AWS Elastic Container Cluster that we need to create.
		
			resource "aws_ecs_cluster" "zephyrecscluster" {
					name = "zephyrecscluster"
			}
		After creating the Elastic Container Cluster i began to work with auto_scalling and launch configuration so that when our
		instances are created they are placed into the cluster.To add the instance to the cluster i had to add user_data which will run
		when the instance is created and when it boots us.
			
			user_data = <<EOF
							#!/bin/bash
							echo ECS_CLUSTER=${zephyrecscluster} >> /etc/ecs/ecs.config
						EOF
		
		After finishing this i got really sick on Wednesday that i could not attend classes on Wednesday and Thursday. I know this is
		effecting my group progress on this project but will work on this during weekend to get my part done as soon as possible.
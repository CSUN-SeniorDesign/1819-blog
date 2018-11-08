---
title: "Project 3 Post 2 - Hayden"
author: "Hayden"
cover: "/img/cover.jpg"
tags: ["Hayden Wilcox", "Zephyr", "Week 8"]
date: 2018-10-19T19:15:13-07:00
draft: false
---

My main task this week was to continue working on the ECS. After last week, I realized that the ECS would be heavily involved with terraform. Because I did not have as much interaction with terraform as my teammates have had from the previous projects, I decided I had to spend some time catching up. To do this, I consulted the documentations written by my teammates about their previous parts.
	The two I focused on as I felt they had the most relevant information to what I would need for making the ECS, were setupawsusingterraform.md written by Neel, and Launch_Config.md written by Daniel. Both documentations were very well done and made me realize that I needed to improve on the way that I write my own.
	After following the documentations closely and communicating with my team I was ready to start on the ECS. The first order of business was to declare it, which was the easiest part. I made a new file called ecs-cluster.tf and included the lines:
	resource "aws_ecs_cluster" "zephyrcluster" {
	name = "zephyrcluster"
	}
After that, I had to remake the ASG file, to suit the needs of our project, which ended up looking like this:
resource "aws_launch_configuration" "ASG" {
	name = "ASG"
	image_id = "ami-00430184c7bb49914"
	instance_type = "t2.micro"
	security_groups = ["${aws_security_group.linux-securitygroup.id}"]
	key_name = "newpair"
	user_data = "#!/bin/bash\necho ECS_CLUSTER='zephyrecscluster' > /etc/ecs/ecs.config"	
	iam_instance_profile = "${aws_iam_instance_profile.ecs_profile.name}"
	lifecycle {
		create_before_destroy = true
  }
}
resource "aws_autoscaling_group" "bar1" {
  name                 = "bar1"
  launch_configuration = "${aws_launch_configuration.ASG.name}"
  vpc_zone_identifier       = ["${aws_subnet.zephyrprs1.id}", "${aws_subnet.zephyrprs2.id}","${aws_subnet.zephyrprs3.id}"]

  min_size             = 1
  max_size             = 2
  lifecycle {
    create_before_destroy = true
  }
}
resource "aws_autoscaling_attachment" "asg_attachment_bar" {
  autoscaling_group_name = "${aws_autoscaling_group.bar1.id}"
  alb_target_group_arn   = "${aws_alb_target_group.zephyralbtg.arn}"
The first part, from “resource "aws_launch_configuration" "ASG" {“ on, was what changed the most. 
The image_id was changed to the ami for ECS-Optimized Linux for US-West-2 where we want our instances. 
user_data = "#!/bin/bash\necho ECS_CLUSTER='zephyrecscluster' > /etc/ecs/ecs.config"	 was added which allows the instance to be picked up by the container cluster
iam_instance_profile = "${aws_iam_instance_profile.ecs_profile.name}" was added to give permissions that would allow the instance and the container cluster to communicate.
Other than that, the ASG file was mostly unchanged from its last project’s incarnation. After finishing with the ASG, I created the task definition which looked like this:
resource "aws_ecs_task_definition" "task1" {
  family = "task1"

  container_definitions = <<DEFINITION
[
  {
    "name": "zephyrdocker",
    "image": "902066483070.dkr.ecr.us-west-1.amazonaws.com/dockerdanieltest:latest",
    "cpu": 10,
    "memory": 500,
    "portMappings": [
     {
       "containerPort": 80,
       "hostPort": 80,
	   "protocol": "tcp"
     }
     ],
     "essential": true
  }
]
DEFINITION
}

	Most of the file is self-explanatory I feel, how much cpu and memory to allocate, which ports to use and the like. The most important part I feel is the image: line. This line directs the ECS to upload a new file to the cluster, in this case, the latest version of our dockerfile from the ECR.

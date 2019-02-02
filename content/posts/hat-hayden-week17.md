---
title: "Hat-Hayden-Week-17"
author: Hayden
date: 2019-02-01T22:07:32-08:00
tags: ["Hayden Wilcox", "HAT", "Week 17"]
draft: false
---

This week, our team began work on modularizing our Terraform code. At first, I struggled to understand how the coding works. The idea seemed simple enough, but converting the resources to modules proved to be difficult to me. However, after consulting someone outside the group, I was able to fully understand the methodology. Effectively, writing a module was the same as writing a resource, with anything that might be changed being left as variables and anything that might need to be utilized by a different module needed an output statement. Unfortunately, by the time I understood how to create modules of my own, a teammate had already modularized much of the other code. I was able to create a module that could be used to create ec2 instances, shown here

src/ec2/main.tf:
resource "aws_instance" "ec2" {	
    ami = "${var.ami}"
	availability_zone = "us-west-2a"
	instance_type = "${var.instance_type}"
	subnet_id = "${var.subnet_id}"
	associate_public_ip_address = true
	vpc_security_group_ids = ["${var.security_group_ids}"]
}

Most of the attributes I made into variables so that they could be changed as needed. The only ones I chose not to were the availability zone being "us-west-2a" as that is where we want all of our instances to be located, and the association of the public ip address to the instance. When called, it currently looks like this:

ec2_instance.tf:
module "ec2_instance" {

	source = "./src/ec2/" 
	ami = "ami-40d1f038"
	instance_type = "t2.micro"
	subnet_id = "${module.vpc_and_ig.autogen_public_subnet_ids}"
	security_group_ids = "${module.nat_sg.security_group_id}"
}

Here, the ami and instance type make this instance a small, free linux instance. The subnet and security group utilize modules created by my teammate Tyler, which allow the instance to be accessed on the internet.

All in all, this week was heavily focused on the modules. Going into next week, there are a number of topics that I realized that I need to gain an understanding of, as they had previously been handled by my other members. One such topic is that of IAM users and roles, since following the dissolution our AWS organization, I will need to be able to allow other users to access my resources. I will also need to learn how states and state-lock work, a useful tool that I have ignored too long that would help greatly with collaboration. Finally, we still need to convert our few ansible playbooks into roles, which I may take on myself during the course of next week.
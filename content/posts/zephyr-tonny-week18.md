---
title: "Zephyr Tonny Week18"
date: 2019-02-08T16:07:11-08:00
draft: false
---
<h3> Tonny Wong: </h3>

	Assigned duties for this week:
	
			- Learn Modules
			- Implement Modules to the following:
			
				- IAM
				- VPC
				- NAT
				- EC2
				- S3Bucket
				
			
		Learning to setup Modules for Terraform files:
		
		Modules in Terraform are self-contained packages managed as a group.
		Modules are reusable components of Terraform and organizes your files to make it simple and understandable.
		
			Theres was little documentation on modules but I have figured out the gist of the files and how it works:
				- My method of using modules will be formed of multiple files:
					- Development
					- Modules
						- Where all the terraform files will be splitted in folders and used.
					- Product
				- Create a terraform file main where you would use to module. Heres an example of a module that will look for the VPC and the EC2 terraform files:
				
								provider "aws" {
									region		= "us-west-2"
								}
								module "my_vpc" {
									source		= "../modules/vpc"
									vpc_cidr	= "172.31.0.0/22"
									tenancy		= "default"
									vpc_id		= "${module.my_vpc.vpc_id}"
									subnet_cidr	= "172.31.1.0/22"
								}
								module "my_ec2" {
									source			= "../modules/ec2"
									ec2_count		=	1
									ami_id			= "ami-0bbe6b35405ecebdb"
									instance_type	= "t2.micro"
									subnet_id		= "${module.my_vpc.subnet_id}"
								}
				
				- The module looks for the source of the file and verifies which one comes first and creates it.
		My module file is still being developed so this is concidered the basics of my understanding in what a module can do but it has potential.
				
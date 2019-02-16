---
title: "Zephyr Tonny Week19"
date: 2019-02-15T22:30:18-08:00
draft: false
---

<h3> Tonny Wong: </h3>

	Assigned duties for this week:
	
			- Implement Modules to the following:
			
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
				- Current Module Main:
				
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
							module "my_s3bucket" {
								source			= "../modules/s3bucket"
							}
							module "my_s3backend" {
								source			= "../modules/s3backend"
							}
				
				- Working on the other files and have been learning AWS module training
				
---
title: "Zephyr Tonny Week20"
date: 2019-02-22T19:14:55-08:00
draft: false
---

<h3> Tonny Wong: </h3>

	Finished assigned task:
			
		- Reading and learning use of Amazon RDS	
		- NAT terraform file
		- Bastion terraform file
		- S3Bucket terraform file
		- Backened terraform file
				
	Working on:
		
		- Implement module main to the terraform files
		- RDS terraform and using modules
		- Documentation for modules
		
				- Current Module Main:
				# Region for AWS 
							provider "aws" {
								region		= "us-west-2"
							}
				# Terraform files set as default in vpc, setup differently in main:
							module "my_vpc" {
								source		= "../modules/vpc"
								vpc_cidr	= "172.31.0.0/22"
								tenancy		= "default"
								vpc_id		= "${module.my_vpc.vpc_id}"
								subnet_cidr	= "172.31.1.0/22"
							}
				# Terraform files set as default in ec2, setup differently in main:
							module "my_ec2" {
								source			= "../modules/ec2"
								ec2_count		=	1
								ami_id			= "ami-0bbe6b35405ecebdb"
								instance_type	= "t2.micro"
								subnet_id		= "${module.my_vpc.subnet_id}"
							}
				$ All setups are in terraform file called through main:	
							module "my_s3bucket" {
								source			= "../modules/s3bucket"
							}
							module "my_s3backend" {
								source			= "../modules/s3backend"
							}
		- Files tested with Terraform init, plan, apply
				- When done terraform destroy so that theres no charge.
		- Amazon RDS - easens set up, operate and scale relational database in the cloud.
				- Resizeable capacity while automating time-consuming administration tasks.


				
				

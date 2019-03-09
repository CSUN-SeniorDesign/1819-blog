---
title: "Zephyr Tonny Week22"
date: 2019-03-08T18:10:37-08:00
draft: false
---
<h3> Tonny Wong: </h3>
	
	Task Completed:
	- Documentation for module
	- ASG Terraform with module
	Incomplete:
		
		- RDS terraform:
		
				- Current Module Main:
					provider "aws" {
						region		= "us-west-2"
					}
					module "my_vpc" {
						source		= "../modules/vpc"
						vpc_cidr	= "172.31.0.0/16"
						tenancy		= "default"
						vpc_id		= "${module.my_vpc.vpc_id}"
						subnet_cidr		= "172.31.0.0/24"
						subnet_cidr1	= "172.31.1.2/24"
						subnet_cidr2	= "172.31.2.3/24"
						availability_zone  = "us-west-2a"
						availability_zone1 = "us-west-2b"
						availability_zone2 = "us-west-2c"

					}
						module "my_ec2" {
							source			= "../modules/ec2"
					#		ec2_count		=	1
							ami_id			= "ami-0bbe6b35405ecebdb"
							instance_type	= "t2.micro"
							subnet_id		= "${module.my_vpc.subnet_id}"
						}
						module "my_nat" {
							source		= "../modules/nat"
							vpc_id		= "${module.my_vpc.vpc_id}"
							subnet_id = "${module.my_vpc.subnet_id}"
						}

						module "my_s3bucket" {
							source			= "../modules/s3bucket"
							bucket			= "potatobucket5"
						}
						module "my_s3backend" {
							source			= "../modules/s3backend"
						}
						module "my_bastion" {
							source			= "../modules/bastion"
							vpc_id			= "${module.my_vpc.vpc_id}"
							subnet_id = "${module.my_vpc.subnet_id_ps1}"
						}

						module "my_alb" {
							source			= "../modules/alb"
							vpc_id		  = "${module.my_vpc.vpc_id}"
							subnets		  = ["${module.my_vpc.subnets}"]
							targetid		= "${module.my_ec2.instance_one}"
							targetid1		= "${module.my_ec2.instance_two}"
						}	
					#	module "my_iam" {
					#		source			= "../modules/iam"
					#	}
					
		- Files tested with Terraform init, plan, apply
				- When done terraform destroy so that theres no charge (successful)
		- Amazon RDS - easens set up, operate and scale relational database in the cloud.
				- Resizeable capacity while automating time-consuming administration tasks.
		- Amazon Auto Scaling group contains a collection of EC2s that share similar characteristics, that also are treated as a logical grouping for the purposes of instance scaling and management.
			- Example of the ASG terraform file:
			
				resource "aws_launch_configuration" "ASG" {
					name = "ASG"
					image_id = "ami-0d427646adc2eaa7a"
					instance_type = "t2.micro"
					security_groups = ["${aws_security_group.linux-securitygroup.id}"]
					key_name = "newpair"	
				lifecycle {
				   create_before_destroy = true
				  }
				}
				resource "aws_autoscaling_group" "bar" {
					name                 = "ubuntunginxdatadog"
					launch_configuration = "${aws_launch_configuration.ASG.name}"
					vpc_zone_identifier       = ["${aws_subnet.Main.id}", "${aws_subnet.PRS1.id}","${aws_subnet.PRS2.id}"]

						min_size             = 1
						max_size             = 2

				lifecycle {
					create_before_destroy = true
				  }
				}
				resource "aws_autoscaling_attachment" "asg_attachment_bar" {
				  autoscaling_group_name = "${aws_autoscaling_group.bar.id}"
				  alb_target_group_arn   = "${aws_alb_target_group.potatoalbtg.arn}"
				}




				
				

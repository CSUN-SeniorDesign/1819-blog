---
title: "Project 1 - Post 1: Neel"
author: "Neel Patel"
cover: "/img/cover.jpg"
tags: ["Neel Patel", "Zephyr", "Week 3"]
date: 2018-09-13T11:05:17-07:00
draft: false
---
<h3>Neel Patel: </h3>

	Tasks:
		- Create 1 IAM user in aws with admin access
		- Install terraform in windows
		- Create VPC
		- Create public/private subnets
		- Create internet gateway
		- Create public route table/Associate public subnets to public route table
		- Create bastion ec2 instance with ssh access only.
	1. Create 1 IAM user in aws with admin access
		- zephyr user was created with programmatic access so I can get the Access key and Secret
		Key to access aws through terraform.
	2. Install Terraform in windows
		- Issue: my Terraform files were not in my environmental variables. After adding the files in
		environmental variables i was able to use terraform through CMD.
	3. Create VPC (172.31.0.0/16)
		- Since I did not have any experience with terraform I decided to create resources one by one
		in terraform and apply them to aws so I know that the infrastructure is being created.
		- One of the first thing that I had to do was create a file (terraform.tfvars) where I can store
		my Access key and Secret key of my zephyr IAM user to access aws through terraform.
		- After that, I created files variable.tf, where I have variables define to be used in my
		provider.tf file, and provider.tf, where first I had to define my provider first, which is aws.
		- After that, i created a vpc called zephyrvpc using aws_vpc resource with a CIDR block /16,
		and apply the changes to aws to see if the VPC was created in aws.
		- Also the first thing I learned when I started to create resources in terraform was to have
		unique logical name so they can be used using interpolation syntax later.
	4. Create 3 public subnets (172.31.0.0/22, 172.31.4.0/22, 172.31.8.0/22)
		- After creating vpc I created three public subnets in provider.tf file one by one.
		- At first, I tried to figure out how to put all three subnets in one resource but was unable and
		decided to create one resource of each subnet.
		- The public subnet are size /22 CIDR block since it was a requirement to have 1024 host
		available in each subnet
				o in our case we only have 1019 host because aws reserves the first four IP address in
				the subnet and the last IP address for broadcast

		- Apply the changes to aws to see if the public subnets was created in aws.
	5. Create 3 private subnets (172.31.16.0/20, 172.31.32.0/20, 172.31.48.0/20)
		- I created 3 private subnets the same way I created public subnets and apply the changes to
		aws.

	6. Create internet gateway
		- After the subnets, I created an internet gateway and attached it to my Zephyrvpc using
		vpc_id and interpolation syntax ${aws_vpc.zephyrvpc.id}.
	7. Create public route table
		- After creating the internet gateway, I create a route table inside zephyrvpc and added a new
		route 0.0.0.0/0, so that instances on public subnets can communicate with internet using
		the internet gateway Zephyrgw. Also I then Associated 3 public subnets to the public table
		route.
	8. Create bastion ec2 instance with ssh access only.
		- I created a new file called bastion.tf to hold my aws_instance resource, aws_security_group
		resource, aws_key_pair resource, and output of bastion public ip.
		- To create my bastion instance I chose Ubuntu 16.04 since I want to practice my Linux more
		than Windows commands.
		- In my aws_instance I had following info:
			o ami = &quot;ami-51537029&quot; // AMI number of Ubuntu 16.04
			o availability_zone = &quot;us-west-2a&quot;
			o instance_type = &quot;t2.micro&quot; //free
			o key_name =&quot;${aws_key_pair.bastion_sshkey.key_name}&quot; //created later in file
			o subnet_id = &quot;${aws_subnet.zephyrps1.id}&quot; //using one of the public subnet
			o associate_public_ip_address = true //assign public ip when created
			o security_groups = [&quot;${aws_security_group.bastion-securitygroup.id}&quot;] //security
			group created to only have inbound access through port 22.
		- I create the security group with inboud(ingress) access though port 22 and
		outbound(egress) rule to access internet.
		- I create a key resource to assign a public key for my zephyr private so that after instance is
		create I can use the private key to ssh into bastion using putty.

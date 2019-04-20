---
title: "Hat Aubrey Week27"
date: 2019-04-19T20:31:11-07:00
draft: false
---

# Managing Secrets in AWS RDS #

There are different ways to pass the desired password securely to an RDS instance. 
1. Transfer the password using plaintext and then have an automated or manual process to change it thru gui or cli.
2. Create a custom resource in AWS cloudformation that will retrieve the password using a lambda function from the Parameter store. The custom resource will then pass it to the RDS resource of cloudformation. 
3. Using terraform, pass an encrypted value of the password. 

I tried to do the number 2 but had many problems creating the custom resource. I was able to create the lambda function that will retrieve the password.

		import os

		import datetime, json, uuid, random, string, base64
		import boto3
		
		
		# Boto3 clients
		kms = boto3.client('kms')
		ssm = boto3.client('ssm')
		
		def lambda_handler(event, context):
		  response = ssm.get_parameter(
		    Name = '/Prod/wordpress/wpdbpassword',
		    WithDecryption=True,
		  )
		  event = response['Parameter']['Value']
		  event = event[event.index("=")+1:]
		  return event

However, my problem is formatting the result based on the requirement of using custom resource in AWS and sending it to pre-signed S3 url. So, I moved on to option 3.

**Option 3:**

1. encrypt the desired password using KMS via aws cli
2. On terraform, create a resource that will decrypt the encrypted string. 
3. Pass the output of the encrypted string to the RDS resource.  
In this way, the password is not written in plaintext in the config file.

		aws kms encrypt --key-id 476badbc-4662-48dc-aec4-70b405aaa456 --plaintext 'tyler-rds-test-password-with-terraform' --query CiphertextBlob --output text
		
		data "aws_kms_secret" "rds-password" {
		  secret {
		    # ... potentially other configuration ...
		    name    = "password"
		    payload = "AQICAHgCqOYfY4je4KdHq/p7/TvVl2HEJkbWBp7mMjB4KtBCbwEQJV94V/zYAhw59kH6OyCXAAAAhTCBggYJKoZIhvcNAQcGoHUwcwIBADBuBgkqhkiG9w0BBwEwHgYJYIZIAWUDBAEuMBEEDBSD8Wt/Dc+d2YljcwIBEIBBc1hcPjkK8NFyv6hUkODIcxPe7lD8DPAc+boXLJ/JxjLCKySm/58MNMpva/Ya+6s+cc04n40LJtUsEjpV94Phiwc="
		  }
		}

		module "rds"{

		source = "./src/rds"
	
			# How much storage, and what type.
			storage_gbs = 10
			storage_type = "gp2"
		
			# What engine and instance to use.
		 	engine = "mysql"
			instance_type = "db.t2.micro"
		
			# Username and password for our instances.
			username = "username"
			password = "${data.aws_kms_secret.rds-password.password}" 
		
			# Name of the database.
			database_name = "wordpress"
		
			# DB subnet group. DB instaces will be placed in these subnets.
			db_subnet_group = "${module.vpc_and_ig.db_subnet_group_name}"
		
			# List of security groups to attach to the RDS instances.
			security_group_list = ["${module.sql_sg.security_group_id}"]
		}



**Leaks**

It is important to know where the leaks will be so that we can make the proper precaution. In doing it this way, the value will not be leaked in the console output of the terraform apply or plan as it will say it is sensitve.

The password will be leaked:  
- in the tfstate file. So, be sure to use remote state file and encrypt the file on rest and audit access to the file.
- in the console after doing the apply procedure, if an output resource was created that displays the password.
---
title: "Brian's Blog Spring Week 4."
date: 2019-02-15T11:44:49-08:00
draft: false;
tags: ["Brian Sandoval", "SAPS", "Week 194"]
layout: 'posts'
---

# Spring week 4 update

## Rundown this week
This week I was able to find a way to create a module for setting up the problem the only issue is that when creating the backup it ends up taking a long time to spin up so for now that is an issue that I have to look into further detail. So far below the backup gets disabled but I am looking to take that part out and see if there is a way to not make the downtime so long for the backup.

### Module Database

module "db" {
  source = "../../"

  identifier = "jackdb"

  engine            = "sqlserver-ex"
  engine_version    = "14.00.1000.169.v1"
  instance_class    = "db.t2.micro"
  allocated_storage = 20
  storage_encrypted = false

  name     = "jackdb"
  username = "user"
  password = "mypasswd"
  port     = "1327"

  vpc_security_group_ids = ["${data.aws_security_group.default.id}"]

  maintenance_window = "Mon:00:00-Mon:03:00"
  backup_window      = "03:00-06:00"

  # disable backups to create DB faster
  backup_retention_period = 0

  tags = {
    Owner       = "user"
    Environment = "dev"
  }

The following functions are used to pull in the information of the vpc along with the subnet and security group ID's. I still need to pull out the information and fill in the holes with configuring our vpc subnets as well as needing to add a more detailed security group.
### Data information

data "aws_vpc" "default" {
  default = true
}

data "aws_subnet_ids" "all" {
  vpc_id = ["${data.aws_vpc.default.id}"]
}

data "aws_security_group" "default" {
  vpc_id = ["${data.aws_vpc.default.id}"]
  name   = "default"
}

All variables end up getting spit out and end up going to another file called variables.tf so that if they are needed to be called on the other functions can call upon them. Some future plans that I have is to still find a way to integrate secrets manager with the snippet of terraform that I have been working with but i still need to find a way to call on the credentials.
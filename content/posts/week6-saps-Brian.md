---
title: "Brian's Blog. Week 6 Spring"
date: 2019-3-1T15:57:18-07:00
draft: false
layout: 'posts'
tags: ["Brian Sandoval", "saps", "week6"]
---
# Week 6 Spring Brian's Blog
## Overview
This week I was able to finish setting up  the last stages of saps rds database along with the implmentation of secrets mangager. Right now we are currently not worried about rotation for rds but we might address to it later. We might end up using our IAM user accounts to be able to map and have access to database users that we will each have for ourselves.
## Future plans
Our future goals is to further test out our rds database make sure that the backup settings are in place. We want to move on ahead as well with some form of monitorinIg. We were thinking of different monitoring systems but we might just end up going forward with datadog due to that we have access to it for two years which is more then enough time. I might end up setting up an rds aurora cluster for test purposes it could prove as a good learning experience.
```
resource "aws_rds_cluster" "default" {
  cluster_identifier      = "aurora-cluster-demo"
  engine                  = "aurora-mysql"
  availability_zones      = ["us-west-2a", "us-west-2b", "us-west-2c"]
  database_name           = "mydb"
  master_username         = "foo"
  master_password         = "bar"
  backup_retention_period = 5
  preferred_backup_window = "07:00-09:00"
}
```
So far this is whaat I have looked into and I am still finding out ways to implement a cluster of course this is all just planning incase we decide to roll out with setting this up.
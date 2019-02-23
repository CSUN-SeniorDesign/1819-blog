---
title: "Brian's Blog Spring week 5"
date: 2019-02-22T12:10:08-08:00
draft: false
tags: ["Brian Sandoval", "Beats", "Week 5"]
layout: 'posts'
---
# Brian's Blog Spring Week 5
## Summary this week
This week i worked on createing a module for RDS so far what i have is below:
```
module "rdsdatabase" {
  source = "terraform-aws-modules/rds/aws"

  identifier = "jackdatabase"

  engine            = "mysql"
  engine_version    = "5.7.19"
  instance_class    = "db.t2.micro"
  allocated_storage = 10

  name     = "demodb"
  username = "${var.myuser.}"
  password = "${var.mypasswd.}"
  port     = "3424"

  iam_database_authentication_enabled = true

  maintenance_window = "Mon:00:00-Mon:03:00"
  backup_window      = "03:00-06:00"
  delete_protection = true
}
```
This portion specifys the initial installation. Identifier creates the name for the database and I went ahead and went with mysql with an allocation of 10 gigabytes for it. By choosing the micro instance class it will be keeping costs at a little bit over a penny per hour. The maintenance window is the amount of time period that we have for down time to be able to push changes and so on. Backup window just specifys when a backup of our database will be created. I am still unsure on what the team wants for back up and maintenance windows but I will follow up with them. I have as well added the option to be able to use our iam user to authenticate us to have access to the database, by doing so the team can have access to their accounts without having to use another user. I have as well enabled delete protection for this database module.

## Obstacles and future plans
The first issue which is a minor one is to create subnets for our database which shouldnt be an issue to add then after attach and map it to our vpc. I will end up creating a resource that will assign the subnets, and Ill just end up importing it after. So far I have:
```
resource "aws_db_subnet_group" "jackdefault" {
  name       = "main"
  subnet_ids = ["${aws_subnet.frontend.id}", "${aws_subnet.backend.id}"]

  tags = {
    Name = "jackdatabase"
  }
}
```
 I am still working to looking to incorporate secrets manager into this database since that is our overall objective for this sprint along with the RDS, I will continue to work on resolving this issue through the weekend. Another obstacle that I have come across is to see whether we will implment some form of monitoring for the our database as well as finding out a way to further build upon secrets manager one its up and running.
 Secrets manager what has been done so far:
 ```
 resource "aws_secretsmanager_secret_version" "queen" {
  secret_id     = "${aws_secretsmanager_secret.queen.id}"
  secret_string = "${var.secretsmanagerinfo}"
}
```
So this should be able to pull out my information in secret string I need to just figure out why it isnt pulling out which most likely be a syntax or logical error but I will look into this further this weekend. After Secrets Manager is figured out I will touch base with my team to see what they further want to build which we might end up implmenting rotation but for that we might have to come up with a lambda function. 
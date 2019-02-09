---
title: "Brian's Blog Spring Week 3."
date: 2019-02-08T12:13:44-08:00
draft: false
tags: ["Brian Sandoval", "SAPS", "Week 3"]
layout: 'posts'
---
# Brian's Blog  Spring Week 3

## Weekly update
This week I have been finishing up the documentation for our current infrastructure. I have as well been looking into setting up RDS for aws currently I have consulted with my team and they reccomend that i figure out a way to use secrets manager. Inititally so far this is what I have about setting up a database instance. The following allows us to initiallize an insatnce by providing the user and password. of the the user/master account we are trying to log in to.

```
resource "aws_db_instance" "jack" {
  allocated_storage    = 20
  storage_type         = "gp2"
  engine               = "mysql"
  engine_version       = "5.7"
  instance_class       = "db.t2.micro"
  name                 = "mydb"
  username             = "user"
  password             = "passwd"
  parameter_group_name = "default.mysql5.7"
}
```
I would end up using variables for the username and password to not display the credentials.

## Future goals
I am hoping to be able to finish testing setting up an rds on my own infrastructure before applying it to our current one. Also I need to setup secrets manager for our vpc.

## Secrets Manager
So far what I have researched about secrets manager is that it I am able to store a password or any login information that is needed so that I am able to access information in a database by being able to not have to type in my credentials for my information within the database or type in my information to ssh into a particular instance. I am learning more about secrets manager but I beleive the way about going about accessing is that I will have to end up setting up an endpoint on our vpc specifically for secrets manager. I will need the below snippet of terraform code so that I am able to access my protected string.

```
resource "aws_secretsmanager_secret_version" "example" {
  secret_id     = "${aws_secretsmanager_secret.example.id}"
  secret_string = "example-string-to-protect"
}

```
I have still a bit of questions to discuss with my team for example there is a way to enable versioning rotation but I have to consult with how the lambda function is coming along or if we will even end up using it. We can as well pull in the version of what we are using if we will be moving forward with versioning

```
version_stage = "example"

```

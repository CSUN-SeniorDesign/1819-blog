---
title: "Matabit Shahed Week 22"
date: 2019-03-08T20:24:14-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 22"]
layout: "posts"
---

# Terragrunt
Going into each directory to deploy the terraform module that we created for the environment is cumbersome, especially if there is a set order of modules to run. 

We contemplated developing a bash script to deploy our entire infrastructure but after some research we found that there are already tools out there to do this.

To automate our infrastructure deployment we are using [terragrunt](https://github.com/gruntwork-io/terragrunt).

# Using Terragrunt

After the environment is setup and all the dependecies are declared, we can run terragrunt either from within the module or from the environment.

Since our goal is to run terragrunt for the entire environment, we are going to run the commands from the environment folder.
(`path/to/prod`) e.g. (`~/projects/csun-saps-infrastructure/terraform/prod`)

Once inside the environment folder you can use the following commands: 
```
terragrunt plan-all
terragrunt apply-all
terragrunt destroy-all
terragrunt output-all
terragrunt validate-all
```

# Switching ECS Launch Types

Last week we got our first bill from AWS with a total of $17.54 after credits had been applied. What was weird is that not all our credits applied. The education credits that we have don't apply to the AWS Fargate launchtype for ECS. After some back and forth with AWS Support, we have been told that if we switch to the EC2 Launchtype for ECS, we will be able to apply our education credits towards our bills.

So as part of our next sprint is switching to ECS.

# Monitoring

Due to our switch to the EC2 Launchtype, we will be able to utilize Prometheus to monitor our infrastructure since we now will have more control over the instances that host our containers.
We did not have this opportunity with Fargate.




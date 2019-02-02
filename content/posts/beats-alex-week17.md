---
title: "Alex's Blog. Week 17."
date: 2019-02-01T13:26:36-08:00
draft: false
tags: ["Alex Enaceanu", "BEATS", "Week 17"]
layout: 'posts'
---

# CIT 481 - SAPS
#### Week 17
This week we had some meetings with the other SAPS teams and made progress on our documentation. Both other SAPS teams were just getting ready to pick things back up. One had personnel changes due to some students graduating (IS) and the other team (CS) was working on their portion.  

I worked on the documentation for the  AWS Application Load Balancer (ALB) which is deployed via code inside a Terraform module. That code specifies the details such as listeners and respective security groups.

One important aspect of documentation for me has been updating the glossary with new terms each time a new service or application is used. The goal is though eventually have the bolded words link to their respective glossary terms. Below is a sample of it:
  # AWS Application Load Balancer (ALB)

  #### Notes

  * Terms will appear in bold the first time they are mentioned in this documentation. They are defined in the glossary file.

  * The syntax for referencing module outputs is ${module.NAME.OUTPUT}, where NAME is the module name given in the header of the module configuration block and OUTPUT is the name of the output to reference.

  * Note that the ALB functions at Layer 7 of the OSI model.

  ## What is an Application Load Balancer (ALB)?
  An ALB is 1 of 3 types of load balancing options available from Amazon Web Services (AWS) An ALB serves as the single point of contact for clients into your network. The ALB distributes incoming application traffic across multiple targets, such as EC2 instances, or Dockerized images in our case, and in multiple Availability Zones. **This increases the availability of your application.** You need to add one or more listeners to your ALB.

  **Listeners** are actively checking for incoming connections requests from clients, using preconfigured ports and protocols. The listener forwards this traffic to one or many predefined **target groups**.

  Each target group routes traffic to registered targets such as Docker images that is specified in rules. Health checks can also be configured.

  Below is the breakdown of the code that deploys the ALB within the AWS environment. Note that there will be one ALB per environment at later stages of the project: Testing, staging, and production.

        resource "aws_lb" "main" {
          security_groups                   = ["${concat(var.security_group_ids, list(aws_security_group.main.id))}"]
          subnets                           = ["${var.public_subnet_ids}"]
          name                              = "alb-${var.environment}${var.name}"
          internal                          = false
          load_balancer_type                = "application"
          idle_timeout                      = 60
          enable_cross_zone_load_balancing  = true
          enable_deletion_protection        = false


  You can see the networking and security group assignments in the Terraform code above. . . .

  ---End Documentation example---

  I also wrote the documentation for the Access Log Bucket which is deployed along side the ALB in the Terraform module. I will continue to work on the documentation throughout the project and constantly refine it.

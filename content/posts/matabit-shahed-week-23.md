---
title: "Matabit Shahed Week 23"
date: 2019-03-14T20:48:40-07:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 23"]
layout: "posts"
---

# Switching from Fargate to EC2

AWS offers two different launchtypes for AWS ECS. One being Fargate, which abstracts away the underlying EC2 architecture, or the EC2 launchtype, which let's you manage the underlying EC2 infrastructure.

As mentioned in the last post we've had to do this switch, since our credits only apply to the EC2 launchtype for ECS. 

# Switching out the module

Since the parameters were mostly the same for when we call the modulem, we mostly only switched out the module itself. We had to add roles, autoscaling groups, a launch configuration, and change the target type of the ALB.

## Troublemakers

A couple of things caused me a lot of headaches. 
1. The user_data part of the launch configuration. I wasn't aware taht we have to add the clustername to the ecs.config of the image that we start. That caused us issues in actually starting the containers on the EC2 instance

```
    user_data = <<EOF
#!/bin/bash
echo ECS_CLUSTER=${var.cluster_name} >> /etc/ecs/ecs.config
EOF
```


2. The STS (Secure Token Service) for some reason was disabled in our region, so we had to manually enable it. This caused us hours of trouble till we figured out that it was a setting in the IAM.

What AWS STS allows us to do, is to give certain services (in this case ECS Serivces) temporary access to IAM user credentials.

3. Finding the correct roles to attach the services was also a difficult endeavour. After some research we found that we needed these two policy documents:

```
data "aws_iam_policy_document" "ecs-service-policy" {
    statement {
        actions = ["sts:AssumeRole"]

        principals {
            type = "Service"
            identifiers = ["ecs.amazonaws.com"]
        }
    }

}
```

```
data "aws_iam_policy_document" "ecs-instance-policy" {
    statement {
        actions = ["sts:AssumeRole"]

        principals {
            type    = "Service"
            identifiers = ["ec2.amazonaws.com"]
        }
    }
}
```
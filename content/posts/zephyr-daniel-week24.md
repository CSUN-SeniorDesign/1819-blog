---
title: "Irrigation Post - Week 24"
date: 2019-03-29T17:14:39-07:00
draft: false
tags: ["Daniel", "Zephyr", "Week 24"]
---

# Week 24
This week I was tasked with creating the ECS cluster for the project. Since we've been using Terraform and that's what I'll be creating our ECS cluster with. This project is a change of pace because we have exchanged roles in this sprint, I haven't used Terraform in quite some time so I'm interested to see how everything is running on that end. We already have Terraform setting up most of our infrastructure so I'll just be piggy backing off what we have.

To begin the ECS cluster I looked at the Terraform Documentation to see what they have about it. It seems that the first step that I would want to do is setup appropriate roles for the ECS cluster within our roles.tf.

```
resource "aws_iam_role" "ecs-instance-role" {
name = "ecs-instance-role"
path = "/"
assume_role_policy = "${data.aws_iam_policy_document.ecs-instance-policy.json}"
}

data "aws_iam_policy_document" "ecs-instance-policy" {
statement {
actions = ["sts:AssumeRole"]

principals {
type = "Service"
identifiers = ["ec2.amazonaws.com"]
}
}
}

resource "aws_iam_role_policy_attachment" "ecs-instance-role-attachment" {
role = "${aws_iam_role.ecs-instance-role.name}"
policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceforEC2Role"
}

resource "aws_iam_instance_profile" "ecs-instance-profile" {
name = "ecs-instance-profile"
path = "/"
role = "${aws_iam_role.ecs-instance-role.id}"
provisioner "local-exec" {
command = "sleep 60"
}
}

resource "aws_iam_role" "ecs-service-role" {
name = "ecs-service-role"
path = "/"
assume_role_policy = "${data.aws_iam_policy_document.ecs-service-policy.json}"
}

resource "aws_iam_role_policy_attachment" "ecs-service-role-attachment" {
role = "${aws_iam_role.ecs-service-role.name}"
policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceRole"
}

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
Referencing a Blog, they suggested using these roles as they will make it so the ECS host doesn't have any issues with credentials. The roles are also applied at the instance level. After finishing this file I ran:
```
terrafrom init
terraform policy
```
I didn't encounter any errors which is wonderful and proceeded forward with running:
```
terraform apply
```
This created the policies and roles for the ECS.

The next step is too setup the ECS task definition. My understanding is that this file will provide the requirements we want out of our containers that it creates. This can also be done inside of an auto-scaling group which allows your instances to be more dynamic and can be scaled up and down. This is something I would definitely want to implement but want to work on more static environment before I make it scalable.

I'm still reading on what is needed to run everything but it seems like I have to modify my file in the ALB.tf to include a section for the ECS and apply security groups.

I'm not quite sure how to do that yet or what the ECS cluster security group should look like. Something I should look into.

# Tasks as of Now
I have to finish the ECS terraform files and see if they work well. There shouldn't be any conflicting issues with what we already have since I'm using what my previous members have setup. If I'm able to finish by this weekend I will then add the Prometheus stuff to our infrastructure to run every time it goes up if we ever destroy it. After that I should be finished with everything and then it would be another rotation for roles in the next sprint.

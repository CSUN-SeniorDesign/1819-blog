---
title: "Erik's Blog Week 19."
date: 2019-02-15T11:23:49-08:00
draft: false;
tags: ["Erik Blomquist", "BEATS", "Week 19"]
layout: 'posts'
---

## Auto Scaling with ECS Fargate and Terraform
We will be creating a target and two policies that will use a step scaling approach to autoscale. We need a monitoring tool such as CloudWatch, Prometheus or ELK to trigger our policies to scale up or down. I will not include the last step because we have not decided which tool to use just yet.

- [Auto Scaling Target](#auto-scaling-target)
- [Auto Scaling Policies](#auto-scaling-policies)
    - [Scale Up](#scale-up)
    - [Scale Down](#scale-down)
- [variables](#variables)

### Auto Scaling Target
We will automatically scale our capacity up or down by 1. It will not go lower than 3 (min_capacity) or higher than 6 (max_capcity). 

Create a new file called `autoscale.tf` and paste the following:

    # Autoscaling target
    resource "aws_appautoscaling_target" "target" {
        service_namespace  = "ecs"
        resource_id        = "service/${aws_ecs_cluster.main.name}/${aws_ecs_service.main.name}"
        scalable_dimension = "ecs:service:DesiredCount"
        role_arn           = "${var.ecs_autoscale_role}"
        min_capacity       = 3
        max_capacity       = 6
    }


### Auto Scaling Policies
We need to add two policies, one for scaling up and for down.

#### Scale Up

    # Scale up +1
    resource "aws_appautoscaling_policy" "up" {
        name               = "saps_scale_up"
        service_namespace  = "ecs"
        resource_id        = "service/${aws_ecs_cluster.main.name}/${aws_ecs_service.main.name}"
        scalable_dimension = "ecs:service:DesiredCount"

    step_scaling_policy_configuration {
        adjustment_type         = "ChangeInCapacity"
        cooldown                = 60
        metric_aggregation_type = "Maximum"

        step_adjustment {
        metric_interval_lower_bound = 0
        scaling_adjustment          = 1
        }
    }

    depends_on = ["aws_appautoscaling_target.target"]
    }

#### Scale Down

    # Scale down -1
    resource "aws_appautoscaling_policy" "down" {
        name               = "saps_scale_down"
        service_namespace  = "ecs"
        resource_id        = "service/${aws_ecs_cluster.main.name}/${aws_ecs_service.main.name}"
        scalable_dimension = "ecs:service:DesiredCount"

    step_scaling_policy_configuration {
        adjustment_type         = "ChangeInCapacity"
        cooldown                = 60
        metric_aggregation_type = "Maximum"

        step_adjustment {
        metric_interval_lower_bound = 0
        scaling_adjustment          = -1
        }
    }

    depends_on = ["aws_appautoscaling_target.target"]
    }

#### Variables
The following variables will be used with our task definition later. Create a new file called `var.tf` and paste the following:

    variable "fargate_cpu" {
        description = "Fargate instance CPU units to provision (1 vCPU = 1024 CPU units)"
        default     = "1024"
    }

    variable "fargate_memory" {
        description = "Fargate instance memory to provision (in MiB)"
        default     = "2048"
    }
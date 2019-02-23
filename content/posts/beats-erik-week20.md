---
title: "Erik's Blog Week 20."
date: 2019-02-22T12:10:08-08:00
draft: false
tags: ["Erik Blomquist", "BEATS", "Week 20"]
layout: 'posts'
---

## How to add CloudWatch Metrics
My last post explained how to add auto scaling to your ECS Services by adding a target and policies to scale up or down. This post will build upon that and explain how to trigger these policies by CloudWatch metrics.

- [CPU Utilization](#cpu-utilization)
- [Memory Utilization](#memory-utilization)
- [Variables](#variables)

### CPU Utilization
This code will be added `main.tf` in your ECS folder. It creates a new Cloudwatch metric that provides information about CPU usage for our service. Once the CPU usage reaches 75%, it will trigger the auto scaling policy to scale up another container.



    # CloudWatch Metric - CPU
    resource "aws_cloudwatch_metric_alarm" "service_cpu_high" {
    alarm_name          = "${var.project}_cpu_utilization_high"
    comparison_operator = "GreaterThanOrEqualToThreshold"
    evaluation_periods  = "2"
    metric_name         = "CPUUtilization"
    namespace           = "AWS/ECS"
    period              = "60"
    statistic           = "Maximum"
    threshold           = "75"

    dimensions {
        ClusterName = "${var.cluster}"
        ServiceName = "${var.service}"
    }

    alarm_actions = ["${aws_appautoscaling_policy.up.arn}"]
    ok_actions    = ["${aws_appautoscaling_policy.down.arn}"]
    }


### Memory Utilization
Similar to the code above but this will add a CloudWatch metric for Memory usage. Once it reaches 75% it will scale up another container.


    # CloudWatch Metric - Memory usage
    resource "aws_cloudwatch_metric_alarm" "memory_utilization_high" {
    alarm_name          = "${var.project}_memory_utilization_high"
    comparison_operator = "GreaterThanOrEqualToThreshold"
    evaluation_periods  = "2"
    metric_name         = "MemoryUtilization"
    namespace           = "AWS/ECS"
    period              = "60"
    statistic           = "Maximum"
    threshold           = "75"

    dimensions {
        ClusterName = "${var.cluster}"
        ServiceName = "${var.service}"
    }

    alarm_actions = ["${aws_appautoscaling_policy.up.arn}"]
    ok_actions    = ["${aws_appautoscaling_policy.down.arn}"]
    }


### Variables
We need to add variables in order for the code above to work properly. Add these two variables to your `variables.tf` file. 




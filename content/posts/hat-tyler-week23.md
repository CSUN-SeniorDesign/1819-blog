---
title: "Tyler's Blog. Week 23."
description: "Datadog + Lambda."
date: "2019-3-14"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 23"]
---

### What's new this week?
This week I worked on getting Datadog monitoring and metrics integrated with our AWS infrastructure. Last sprint, I set up the Datadog agent for our ECS clusters. This sprint, I set up a Datadog IAM role (with an external account), and set up a Datadog Lambda function for log collection.

#### Datadog AWS Account Integration
On the Datadog website, I manually linked my AWS account. I received an AWS External Id, which I used in an IAM role as an external ID, with the principal being Datadog's account. This will allow the Datadog account to have some access to my AWS account. With this alone however, the Datadog role isn't given any permissions.

Next, I created an IAM policy and policy attachment with Terraform to add permissions to the linked Datadog account. This allows the Datadog account to receive information such as ec2 instance number and billing information. At this point, we enter the AWS role name on Datdaog manually, and dashboards are automatically created to monitor the applicable infrastructure.

#### Datadog Log Collection with AWS Lambda
Next, I wanted to set up log collection using AWS Lambda. To do this, I used Datadog's official lambda function that's hosted on Github. I created a Lambda module in Terraform, and zipped the Python file into a zip. This file gets uploaded to AWS Lambda when the infrastructure is created, then can be manually tested. Once the function is on AWS, it will ship different log information from AWS to Datadog (ELB, S3, CloudTrail etc).

All new modules created for metrics are documented in the /docs/ folder on our private repository.

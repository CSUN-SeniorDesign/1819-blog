---
title: "Matabit Shahed Week 13"
date: 2018-11-23T12:38:26-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 13"]
---

# Getting Started With Infrastructure

This week we finally begun building the infrastructure with Terraform, we're starting off with the VPC. We have gotten some requirements as to how we're supposed to set up the subnets.

So here's how we're going to define them:

- Two availability zones (A & C)
- 2-Layer Setup (Private & Public Subnets)
- Three Environments (Prod, Staging, Dev/Test)

This leads us to have the following structure:

### Availability Zone: A

- layer1-dev-us-west-2-a (10.0.0.0/26) - Public
- layer1-staging-us-west-2-a (10.0.0.32/26) -- Public
- layer1-prod-us-west-2-a (10.0.0.64/26) - Public
- layer2-dev-us-west-2-a (10.0.0.96/26) - Private
- layer2-staging-us-west-2-a (10.0.0.126/26) - Private
- layer2-prod-us-west-2-a (10.0.0.160/26) - Private

### Availability Zone: C 

- layer1-dev-us-west-2-c (10.0.0.16/26) - Public
- layer1-staging-us-west-2-c (10.0.0.48/26) - Public
- layer1-prod-us-west-2-c (10.0.0.80/26) - Public
- layer2-dev-us-west-2-c (10.0.0.112/26) - Private
- layer2-staging-us-west-2-c (10.0.0.144/26) - Private
- layer2-prod-us-west-2-c (10.0.0.176/26) - Private

This will give us enough IP addresses's and subnets to build our infrastructure properly with enough containers.

Additionally, we are implementing two gateways:
1. Internet Gateway
2. NAT Gateway

We need these two gateways so that there is proper communication to the internet from both the public and private subnets.

Route tables are setup so that the private subnets can communicate outward to the internet through the NAT Gateway. 

All traffic TO the private subnets is going to happen through an Application Load Balancer.


# Waiting for Test Data

Since we're not allowed yet to play with the official data set (Level 1 Student Data), we are waiting for Leo to create a database with test data as well as a test API. 

Since credentials have to be stored in an AWS Secret Managers, the Computer Science team needs those credentials to properly start working on the application.

Our goal is to develop the infrastructure and support the Computer Science team and our main goal is to build them a CI/CD pipeline. To do that, we need to be able to properly replicate an environment for them.
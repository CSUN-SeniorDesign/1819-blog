---
title: "Sandbox Worms Yerden Week27"
date: 2019-04-19T22:50:10-07:00
draft: false
Categories: [AWS]
Tags: ["Yerden Zhursinbek", "Sandbox Worms", "Week 27"]
---
This week I spent some time catching up on old assignments, as well as researching on tasks required for project and practicing, while dealing with 2 midterm and other projects on a side.  After dealing with dockerfile as individual challenge, I started researching on Elastic Load Balancer since it seemed to be the main focus in the next step. 

Last semester we used Application Load Balancer which had slightly different configurations, but in our project ELB has to be set up to listen ports 80 and 443, while forwarding only htrough port 80 (to the EC2 instances)
First of all ertificate had to be added to Amazon


    resource "aws_iam_server_certificate" "certificate" {
    name      = "certificate"
    certificate_body = "${file("#.crt")}"
    private_key      = "${file("#.key")}"
    certificate_chain = "${file("#.pem")}"
    }

After that load balancer is created and configured as ELB along with security groups and public subnets

    resource "aws_elb" "ELB" {
    name               = "ELB"
    internal           = false
    load_balancer_type = "elastic"
    security_groups    =    [""]
    subnets        =    [""]
    }
Then target groups should be configured for port 80, then 2 listeners should be confifured.
Looking forward to test and run it and report any issues.

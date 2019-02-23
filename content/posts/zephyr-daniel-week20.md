---
title: "Irrigation Post - Week 20"
date: 2019-02-22T15:54:48-08:00
draft: false
tags: ["Daniel", "Zephyr", "Week 20"]
---

# Week 20

Week 20 I continued messing with Datadog and seeing what were our options for monitoring.  After installing a Datadog Agent into our Ubuntu I had changed the configuration file which was located at :

      /etc/datadog-agent/datadog.yaml

This file contains what the Agent will be able to do on the server it's currently in. On the Datadog documentation the Agent for Ubuntu is capable of doing live process monitoring. This is achieved by changing a line in the configuration file:

      process_config:
        enabled: "true"

This will enabled the agent to start live process collection. After changing that value and restarting the agent I went to the Dashboard to view any information I may be receiving. It worked but it's not really giving us information that seems to be of use. I want to continue to work on the agent but it seems easier to use the Agent integration tool for AWS instead of directly installing it on Ubuntu. The difference between the two is the AWS Agent will be using a Lambda function and IAM roles to be setup.

Ill be trying the AWS integration method and see what information we are able to monitor with the Agent through AWS. According to the Documentation we essentially can monitor every aspect of our container such as ECS, EC2, Route 53 and VPC.

# Task as of Now
I want to continue with reading the configuration file that is on Ubuntu and seeing what else I can enable to get more monitoring information. I then want to test the AWS integration and seeing the information we can pull from that as well. Afterwards fix the pipeline and get that up and running.      

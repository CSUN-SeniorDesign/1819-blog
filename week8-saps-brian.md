---
title: "Brian's Blog. Week 8 Spring"
date: 2019-15-1T15:57:18-07:00
draft: false
layout: 'posts'
tags: ["Brian Sandoval", "saps", "week8"]
---
# Brian's Blog  Spring week 8
## Overview
This week was a bit problematic due to the fact that we had to switch from fargate to EC2 so we had to change the way our infrastructure was set up. Shahed took the lead in figuring out the hardest part of converting our terraform code to go based off ec2. I still have RDS currently installed runnning on its own right now on an insance but I havent been able to fully test since we were finishing up the migration to ec2. We were finally able to finish the migration up today seeing that it is the end of the week I can finally be able to start working on my part. I have as well installed prometheus and played around with a few things with it but notihing to serious in terms of how it will have an impact on our infrastructure. Below is an example of how I can attempt to setup the node exporter and the mysql exporter which is essential to setting up:
```
cat << EOF > /opt/prometheus/prometheus.yml
global:
 scrape_interval: 5s
 evaluation_interval: 5s
scrape_configs:
 - job_name: linux
 target_groups:
 - targets: ['localhost:3306']
 labels:
 alias: db1
 - job_name: mysql
 target_groups:
 - targets: ['localhost:3308']
 labels:
 alias: db1
EOF
```
## Future Plans
I want to be able to take advanage of some of the time off next week to be able to test out RDS fully while as well implment more of prometheus and grafana so that I can have some metrics intake. I want to be able to take a look at autoscaling help Erik out so that we can see how efficient our infrastracutre is managing it's resources. I as well want to see if there is a way where we can start setting up a firewall or have some way of trying to secure as best as possible. I as well want to develop an ansible play book that will I will setup in an RDS instance so that it can setup some personalized security settings.
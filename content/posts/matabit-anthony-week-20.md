---
title: "Matabit Anthony Week 20"
date: 2019-02-22T09:01:11-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 20"]
layout: 'posts'
---
# Week 20
I didn't spend much time on the project this week; my main focus was studying for the AWS Solutions Architect certification via A Cloud Guru. I attempted to fix the LDAP issue, but it seems like the culprit might be the ALB. Even with the LDAP ports, open I still receive a 504 Gateway error. As of now, I reverted back to an EC2 instance. 

## Looking into metrics
As of now the ELK stacks gathers and visualizes logs. Metrics are another ballpark; metrics shows information of a host and has the ability to alert when things go wrong such as high CPU usage. I could add metricbeat to the ELK stack but I would have to pay for an enterprise license for alerting. Yelp offers a plugin for alerting but, I want to get my hands on Prometheus and Grafana. 

There's plenty of docker implementations of Prometheus and Grafana using [docker](https://github.com/stefanprodan/dockprom), so I'll give it a try
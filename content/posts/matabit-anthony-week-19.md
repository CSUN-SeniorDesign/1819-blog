---
title: "Matabit Anthony Week 19"
date: 2019-02-17T21:31:07-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 19"]
layout: 'posts'
---
# Week 19
I dove deeper with my ECS cluster progression. This week I focused on the cluster and dynamic port mapping, the resilience, and fixing any issues that came along. LDAP was further integrated by adding validation; which in turn broke the web application in its cluster form; putting the application on an EC2 was perfectly fine. Also, the ELK monitor server randomly broke so I had to troubleshoot that issue. 

## Laravel and session
When using authentication with Laravel, there are multiple methods of the frameworks store sessions. By default, they are stored in the `storage/` directory inside the project's directory. This is why you need special permissions for `storage/` and `bootstrap/cache`. I originally had two `tasks definitions` for each service; production and dev respectively in AZ us-west2-a and us-west2-b. Due to the ALB offloading traffic between the two AZs and containers the sessions would not match up on the users end; which would cause and the error `419 sessions expired`. I scaled down to 1 container per task to test out sessions. My other issue now is when a user logs in, Laravel talks to our LDAP server, or attempts too. The page hands on the `POST` request and times out with the `504 gateway error`. I quickly swapped over to an EC2 to test out validation and it worked perfectly fine. 

## Sprint planning
This weeks sprint planning went well. With our refocus on our application, we narrowed down the most vital task for our project finalization in 9 weeks. 

## ELK
I noticed on Wednesday my ELK server stopped receiving logs. I looked into the elasticsearch ingest history and this has been an issue since Monday. I tried to fix the issue but instead and blew the server away and ran my ELK playbooks; which worked great to bring the stack up in less than 5 minutes. Everything worked fine after that!

## Upcoming sprint
I want to find the resolution for my ECS cluster issue. Laravel also stores sessions in a database, so I will investigate how that would work out. It makes valid sense to store sessions in a database as cluster lifetime is short. After I'm going to play around with metricbeat to gather metrics on my systems and addon alerting.
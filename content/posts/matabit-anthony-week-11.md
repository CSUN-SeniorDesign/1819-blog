---
title: "Anthony Week 11"
date: 2018-11-09T14:23:35-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 11"]
layout: 'posts'
---
# Week 11
This week was relatively short in terms of the workload. I was able to fully meet with my CompSci Group about the project. I joined the Classroom Profiles group that is in charge of creating an application targeted towards students. Although we are in the planning phase of development, I was able to get some work done on the infrastructure side. We met with Steve on Wednesday to gather more information with the current resources we have such as APIs, mockups, and ideas. The web application itself will guide students to their classes and create routes based on their schedule. Another "nice to have" feature is a heat map based on location and number of students during a specific time. This will help with metrics on students' location during school-time so marketing can target students in the most dense areas. 

## Infrastructure planning
I'm the only Ops person for this team so all the infrastructure design and implementation will be handed on my end. This week I planned out the base infrastructure but I'll still need more information from the devs as time progresses. So far, I have the base VPC with the subnets and NAT gateways hashed out. The code will be hosted on ECS clusters in EC2s. Fargate was far too expensive in the long run. I was also able to get the team signed up for AWS educate so we can pool in our credits to pay for AWS. 

## Docker
I've also created a docker container for development based off a Debian image. The Dockerfile is shown below

```Dockerfile
FROM php:7.3-rc-apache

# Install php extensions
RUN docker-php-ext-install pdo_mysql \

# Enable apache modules
  && a2enmod headers rewrite \

# Add user local dev env groups to www-data group
  && usermod -u 1000 www-data && usermod -G staff www-data

# Add apache.conf file
ADD apache.conf /etc/apache2/sites-enabled/000-default.conf

# Expose port 80
EXPOSE 80
```

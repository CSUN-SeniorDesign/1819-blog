---
title: "Matabit Shahed Week 12"
date: 2018-11-15T20:05:29-08:00
draft: false
layout: 'posts'
tags: ["Shahed Salehian", "Matabit", "Week 12"]
---

# Meetings, planning and finally some code!

This week we finally took a step in the right direction. We finally gained access to the code base so that we can play around with it and plan out what we need to do next and we have our requirements and restrictions as well.


## Meeting With Leo
Meeting with Leo from the SAPS team has helped us gain an understanding of what the current architecture looks like and so far it seems to be pretty messy and brittle.

Apparently, the application seems to be built on a Linux machine and then deployed on a Windows T2.Micro instance, because it has the necessary DLL's to run the .NET Core application.

That is not optimal. Our goal, working together with the CS Team is to make sure that the entire system is able to be built and run on a Linux machine. 

Leo has told us that we have a budget of around $400/month which should be plenty to create a highly-available and redundant system. We gathered the minimum requirements of what is needed and have been given some wiggle room to improve ontop of that. 

## The Codebase

Looking at the codebase it is obvious that it's going to take us some time to understand how this .NET application has come together and what we need to do to automate the deployment process. So far we've only automated the deployment of a static-page blog. Managing a web application with a back-end is a little different, but with some time we should be able to figure that out as well.

## The Plan

Our goal is to containerize the application with docker and use AWS ECS with AWS CodePipeline, AWS CodeBuild and AWS Deploy to actually build a CI/CD pipeline that helps us deploy the containers in their clusters.

We need three different environments (services) for testing, staging and production. Considering that we want at least some redundancy, we need at least two tasks each, and have them be load balances. So in total we need at least 6 containers for all the environments together. We still need to do the calculation but it seems that it's going to be cheaper to use the EC2 launchtype instead of Fargate.

However, the pros and cons still have to be determined and a final decision on that has to be made.


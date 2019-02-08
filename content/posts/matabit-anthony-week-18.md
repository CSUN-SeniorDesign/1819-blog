---
title: "Matabit Anthony Week 18"
date: 2019-02-08T09:05:37-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 18"]
layout: 'posts'
---
# Week 18 recap
This week was a push to use an ECS cluster for the service infrastructure. I opted for using an EC2 spot instance due to cheaper costs and the ability to monitor the cluster host; this also gives me the ability to integrate alerting. We also had a meeting with Steve (Wiegly also joined) about the progress of our application with some much-needed feedback. There was a task that had to be done before our presentation on Tuesday, but the assigned individual didn't do the task, so I had to overtake it. The task was to revamp the whole application to use Metaphor V2. There was a lot of merge conflicts that we're a result of the overhaul which had to be dealt with before the presentation. 

## ECS cluster
The next "version" of my service infrastructure was to move to ECS cluster; this allows for continuous deployment which makes my life easier (also brings value to the devs). There's a large number of components to create an ECS cluster. This includes ASG, IAM roles, ALB, and ECS/ECR resources. For more info view this [folder](https://github.com/digitalsoba/classroom-profiles-ops/tree/master/terraform/ecr_ecs) 

### ECS cluster issues
Currently, the cluster works in terms of the ALB pointing to the correct cluster. The issue is the `app.js` and `app.css` are loaded with `HTTP` which throws a mixed content blocked warning. This doesn't load the files which make the page seem broken. I'm currently searching for a fix, so far changes to the `.env` to force `https` don't resolve the issues. 

## Metaphor V2
The original web application was based off Metaphor v1, which was old and didn't take advantage of flexbox. The newest version of Metaphor is built on top of Bootstrap 4. My tasks were to overhaul our UI and match the new tags and classes used by v2. Since Metaphor is a node package rather than a CDN, I constantly have to run `npm run dev/prod` to build the additional assets. I can run `npm run watch` to auto-compile when needed. While rushing to iron out the new design I fixed any merge conflicts when doing pull requests. 

## What's next
* Fixing ECS cluster asset issue
* More development
* Add alerting and metric to the ELK stack
* Test Infrastructure resilience 
* Documentation
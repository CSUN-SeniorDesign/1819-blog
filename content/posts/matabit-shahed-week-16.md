---
title: "Matabit Shahed Week 16"
date: 2019-01-24T17:18:04-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 16"]
layout: 'posts'
---

# So the semester begins...

After a long and refreshing break it is time to get back into our CSUN SAPS project.
Our goal for this current sprint is to document all of the code that we've written so far. From the Dockerfiles, AWS resources and modules to the Gitlab-CI configuration.

My task for this upcoming sprint is to document the EC2 Container Service (ECS) infrastructure that we've implemented in AWS.

Since this project is going to be handed off to a future team, and that team might not have the expertise to understand everything from scratch, we are ensuring that some basic explanations for the terms that are important for the service are explained and how the essential parts of the service are structured.

In ECS it is very important to have a solid understanding of what clusters, serivces and tasks are since a container service is made up of these three resources.

I will not go into detail about the explanations and definitions in this blog post, but I will show some excerpts of the code.


# Task Definition

A Task definition defines the structure of the container that is about to be deployed in one of the services. Each service can have multiple replicas of a task. A Task isn't necessarily just a single container. A task could be mutliple containers that are connected together over a network adapter, which then can be replicated to the desired amount of tasks and then these tasks can be used in conjuction with an application load balancer to distribute the incoming traffic.

Here's what our current task definition looks like for the Node.js Application

```
[{
  "name": "app",
  "image": "563345143758.dkr.ecr.us-west-2.amazonaws.com/saps-prod:latest",
  "essential": true,
  "portMappings": [
    {
      "containerPort": 3000,
      "hostPort": 3000,
      "protocol": "tcp"
    }
  ]
}]
```

Due to us using Fargate as the ECS launch type, we have to match the container port and the host port as this is one of the requirements stated by the Fargate launchtype.


# Static Assets

One element of this project that is on the horizon for us, is static assets. We need to figure out how we want to serve the static assets in this project. As of right now, they're all being served through the application server, however, it would be preferable to offload the static assets to improve performance and load times.



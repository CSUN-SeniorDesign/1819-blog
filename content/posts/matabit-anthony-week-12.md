---
title: "Matabit Anthony Week 12"
date: 2018-11-17T13:06:54-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 12"]
layout: 'posts'
---

# Week 12 
This week was was still still filled with meetings. The Devs and I got together to sign a contract for the project and plan on wire-framing Monday. On the operations end, I begin creating the base for the AWS infrastructure. This includes the VPC, Route53, and S3 backend. I've also testing out DroneCI and plan on using it for our CI/CD pipeline.

## AWS
For the the base VPC, i basically followed the guidelines from project 2. I opted for two private/public subnets, 1 NAT instance, and the appropriate route tables. 

## Drone
I began to play around with DroneCI. I'm quite impressed with the speed and feature-set. They just released their 1.0-RC version with new documentation and setup, which was a breeze. The whole system runs in docker, including its workers. I set it up on a Lightsail server for now just so I can play around with it. I used a simple docker run command to start it up but I plan on converting it to a docker-compose file. 

```docker
docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/var/lib/drone:/data \
  --env=DRONE_GITHUB_SERVER=https://github.com \
  --env=DRONE_GITHUB_CLIENT_ID=${SECRET} \
  --env=DRONE_GITHUB_CLIENT_SECRET=${SECRET} \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=${SECRET} \
  --env=DRONE_SERVER_PROTO=https \
  --env=DRONE_TLS_AUTOCERT=true \
  --publish=80:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:1.0.0-rc.1
```

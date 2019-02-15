---
title: "Tyler's Blog. Week 19."
description: "CI-CD Progress"
date: "2019-2-15"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 19"]
---

### What's new this week?
This week has been primarily focused on improving our CI-CD pipeline and allowing it to work with our new Terraform modules, docker-compose and our new Wordpress code. Originally, this was planned to only take a couple days, but the more I worked on the pipeline, the more I realized how many changes have to be made to get everything working properly.

### CI-CD

Here is a description of some of the improvements, problems and potential solutions I have encountered this week.

### Progress
##### ECS-CLI
* Learned ECS-CLI basics to create a new profile and cluster.

##### ECS
* Working volumes partially set up (works on local machine).
* Suspected communication problems between ECS Wordpress and MySQL server were fixed using environment variables in docker-compose.yml (on local machine).

##### Docker-Compose and CI-CD Pipeline
* docker-compose.yml works on local machines, with working volumes.
* Switched to using alpine images for our GitLab CI runner.
* Converted base and main Dockerfiles into a single docker-compose.yml.
* Removed need for GitLab registry by reducing CI-CD pipeline to a single stage.
* CI build time reduced 90% from original deploy and build time.

### Problems
##### ECR, ECS, Volumes, ECS-CLI, Docker-Compose.
* ECR isn't required for docker-compose, meaning previous ECR code wasn't required.
* ECS-CLI is required to deploy our docker-compose.yml, as it isn't possible to use ECR.
* Docker-compose version 3.3 doesn't work with ECS-CLI. Had to downgrade to version 3.
* Currently creating an ECS task definition during CI-CD Pipeline using ECS-CLI (not using Terraform).
* ECS requires new task definition to work with docker-compose. Previous version only worked with normal Dockerfile.
* Running docker-compose on local machine doesn't emulate AWS architecture (can be issues on AWS that aren't on the local machine).
* Not sure how containers interact with each-other on AWS, if ports must be configured etc.
* ECS Wordpress container stops with error code 1 after around 30-45 seconds of the task running when adding volumes.
* Volumes may require AWS EFS to persist if a container instance is stopped.

##### Monitoring and Logging
* ELK stack recommends 4-8GB of RAM, which would require a $40/month instance. As a result we have decided to use Prometheus + Grafata.
* Prometheus + Grafata will have to be pushed back to next sprint, due to ECR/ECS challenges.
* Logging + Metrics may require removal of ASG, ALB to lower costs (creating 1 server for metrics/logging, 1 server for our container instance).

### Potential solutions and work next week.
* Create the task definition using Terraform instead during the CI build. Allow CI-CD to update the task definition but not create it.
* Determine why ECS Wordpress container is stopping after running for around 30-40 seconds. May need to set up logging which currently isn't in place.
* Completely set up ECS/ECR, and allow it to work with our CI-CD pipeline.
* Set up EFS if required.
* Other challenges if/when they arise.

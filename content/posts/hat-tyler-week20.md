---
title: "Tyler's Blog. Week 20."
description: "CI-CD Complete"
date: "2019-2-22"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 20"]
---

### What's new this week?
My work this week focused on getting the pipeline running, along with getting things ready for the presentation. I attempted to get the CI-CD pipeline working using docker-compose, but we eventually switched back to regular Dockerfiles with ECR and ECS.

### CI-CD Completion
The following is my CI-CD Pipeline code for our repository:

```
image: docker:latest

services:
- docker:dind

stages:
- base

base:
only:
  - master
  - Updating-CI
stage: base
script:
  - apk add py-pip
  - apk add curl
  - apk update
  - pip install awscli
  - cd /builds/tklusz/childsafelegal/Dev/wordpress
  - docker build --rm=true -t build .
  - docker tag build:latest 507963158957.dkr.ecr.us-west-2.amazonaws.com/wordpress:latest
  - $(aws ecr get-login --no-include-email --region us-west-2)
  - docker push 507963158957.dkr.ecr.us-west-2.amazonaws.com/wordpress
  - cd /builds/tklusz/childsafelegal/Dev/wordpress/db
  - docker build --rm=true -t wp_build .
  - docker tag wp_build:latest 507963158957.dkr.ecr.us-west-2.amazonaws.com/db:latest
  - docker push 507963158957.dkr.ecr.us-west-2.amazonaws.com/db

```

The Dockerfiles themselves were created by Aubrey last weekend.

We decided to switch to an alpine based image to save time for builds, eventually I settled on using the docker:latest image, as it already includes Docker.

I was getting errors using the docker:latest image, as it wouldn't allow me to run "docker build" (docker in docker). This is because the CI-CD pipeline itself is inside a docker container on the GitLab servers. In order to get docker in docker working, I had to add the docker:dind service.

We are using 2 separate ECR repositories for our Docker images, one is for the Wordpress and other is for our database. This is to separate the images easily, and to avoid confusion between images. Also, both images can be tagged with the "latest" tag, in their respective repositories.

The build time on the new code has improved over old pipeline. Before this sprint, our build time was 5 minutes for the base image, and 5 minutes for the main image (which would be pushed to ECR). Now, the build time is 3:30, which is a third of the original time. If we eventually could get docker-compose working, our build time could be reduced as low as 1-2 minutes.

 I haven't explained this before, but on AWS there is a Gitlab-Bot user which is used to deploy the CI-CD pipeline. The credentials of the user are set up as environment variables in the CI section, and this user has minimal permissions.

 In future, our images can be tagged with the commit SHA to allow for a staging and production site.

Another advantage of this system is that all infrastructure code still remains in Terraform, instead of having to use ECS-CLI. Also, the website is now actually working after almost an entire sprint's worth of progress.  

### Challenges
I encountered numerous challenges this week.
* Docker-Compose refused to work, even after around 25 hours of trying. As a result, the work I did on it last week wasn't used.
* Our CI-CD pipeline had to be rebuilt again as we switched from a docker-compose file to two Dockerfiles.
* EFS will still need to be set up in future to allow for data persistence.
* The assignment on Wednesday took time away from working on this project, although it only took an hour and a half, if future projects are more complex they could take even more time away from the project.

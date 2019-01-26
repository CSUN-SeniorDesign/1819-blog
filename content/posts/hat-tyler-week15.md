--- 
title: "Tyler's Blog. Week 15." 
description: "CI/CD Pipeline Stages" 
date: "2018-12-5" 
categories: [ 
    "Blog",
] 
menu: "main" 
tags: ["Tyler Kluszczynski", "HAT", "Week 15"]
---

#### What's new this week?
This week, I made improvements to the GitLab CI. Specifically, changing the number of stages in the pipeline from 1 to 2. The gitlab-ci.yml file has also been changed, along with the two scripts.  

#### New .gitlab-ci.yml
This will be explained in the pipeline stage sections, but it is here for an overview of the new code.

```
stages:
  - base
  - main

base:
  only:
    - Update-Base
  stage: base
  image: ubuntu:18.04
  script:
    - chmod +x /builds/tklusz/childsafelegal/DevOps/CI/Base.sh
    - cd /builds/tklusz/childsafelegal/DevOps/CI/
    - ./Base.sh
  
main:
  only:
    - master
    - Updating-CI
  stage: main
  image: registry.gitlab.com/tklusz/childsafelegal:base
  script:
    - chmod +x /builds/tklusz/childsafelegal/DevOps/CI/Main.sh
    - chmod +x /builds/tklusz/childsafelegal/DevOps/CI/Deploy.sh
    - cd /builds/tklusz/childsafelegal/DevOps/CI/
    - ./Main.sh
    - ./Deploy.sh

```

#### First Pipeline stage.
The first stage of the pipeline is the "base" stage. This stage runs the Base shell script. This shell script has changed from the previous version, and will change again next semester.

Base.sh:

```
#!/bin/bash

# Moving production code to the Docker directory.
mv -v /builds/tklusz/childsafelegal/Dev/Build /builds/tklusz/childsafelegal/DevOps/Docker/Main

# Updating the container.
apt-get update

# Installing docker, python-pip, awscli
apt-get install -y docker
apt install -y docker.io

# Starting docker
service docker start

# Moving to base dockerfile directory then building the base image.
cd /builds/tklusz/childsafelegal/DevOps/Docker/Base
docker build --rm=true -t registry.gitlab.com/tklusz/childsafelegal:base .

# Logging into GitLab registry
docker login registry.gitlab.com -u tklusz -p $ACCESS_TOKEN

# Pushing to Registry.
docker push registry.gitlab.com/tklusz/childsafelegal:base

```

Note that we are now using an access token to login to the GitLab registry, to push this intermediate image into the container registry. That is because this base image is only created whenever we need to update software on the backend (such as new OS or software versions).

#### Second Pipeline stage.
The next stage is the stage that does the configuration on the image, then uploads to AWS ECR. First, the Main.sh script is ran to start docker, pull the image from the GitLab registry, then run the main dockerfile.

Main.sh:

```
#!/bin/bash

# Moving builds to docker repo.
mv -v /builds/tklusz/childsafelegal/Dev/Build /builds/tklusz/childsafelegal/DevOps/Docker/Main

# Start docker daemon.
service docker start

# Login to registry
docker login registry.gitlab.com -u tklusz -p $ACCESS_TOKEN

# Moving to main dockerfile directory then building the main image.
cd /builds/tklusz/childsafelegal/DevOps/Docker/Main
docker build --rm=true -t build .

# Tagging our build with the commit SHA.
docker tag build:latest 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr:$CI_COMMIT_SHA

```

After this, the Deploy.sh script is ran, which is the same as in last week's blog.

#### Dockerfiles
The base dockerfile has been slightly updated, and reduced the number of container layers by using && operators.

Base Dockerfile:

```
FROM ubuntu:18.04

RUN echo 'debconf debconf/frontend select Noniterative' | debconf-set-selections

RUN apt-get update && apt-get install -y docker apache2 python-pip && pip install awscli
RUN apt install -y docker.io && apt-get clean

```

The main dockerfile is the same, except we are now using the registry.gitlab.com in the FROM line, as an intermediate container (sent to the registry by the base image).

#### Future work on Docker and CI
Next semester, my main goal for the Docker and CI is to improve the build time. As of now, building both the base and main image takes 10 minutes total, and building just the main image takes 5 minutes. I hope to reduce this by using Alpine instead of Ubuntu to run our server instance and as the image being ran in the containers by the GitLab runners.

The CI will also need to be updated to create more resources, such as a PHP server, and to move specific files to certain directories (for example, moving PHP code from the repo to PHP server directory), once the developers provide the code to us. 

In the farther future into next semester, I would like to gain experience in a service such as Docker Swarm or Kubernetes. Our client may also require the CI to be moved to it's own hosted service (such as hosting our own runners, or running a Jenkins server) if the project expands further.

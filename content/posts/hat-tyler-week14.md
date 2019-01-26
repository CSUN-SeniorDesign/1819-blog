--- 
title: "Tyler's Blog. Week 14." 
description: "Improving Docker and CI" 
date: "2018-11-28" 
categories: [ 
    "Blog",
] 
menu: "main" 
tags: ["Tyler Kluszczynski", "HAT", "Week 14"]
---

#### What's new this week?
I've been improving the Docker and GitLab CI for better organization and maintainability. It is also documented better in-line.

#### The new .gitlab-ci.yml
This is now in the /DevOps/CI/ directory, as it allows us to organize all of the CI code, and keeps it out of the main directory. It is also much more cleaner. Here is the old version:

```
image: ubuntu:18.04

stages:
  - build

get_code:
  only:
    - master
  stage: build
  script:
    - mv -v /builds/tklusz/childsafelegal/Dev/Build /builds/tklusz/childsafelegal/DevOps/Docker/Main
    - apt-get update
    - apt-get install -y docker python-pip
    - apt install -y docker.io
    - service docker start
    - cd /builds/tklusz/childsafelegal/DevOps/Docker/Base
    - docker build --rm=true -t base .
    - cd /builds/tklusz/childsafelegal/DevOps/Docker/Main
    - docker build --rm=true -t build .
    - docker tag build:latest 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr:$CI_COMMIT_SHA
    - pip install awscli
    - $(aws ecr get-login --no-include-email --region us-west-2)
    - docker push 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr

```

And here is the new version:

```
image: ubuntu:18.04

stages:
  - build

main:
  only:
    - master
    - Updating-CI
  stage: build
  script:
    - chmod +x /builds/tklusz/childsafelegal/DevOps/CI/Base.sh
    - chmod +x /builds/tklusz/childsafelegal/DevOps/CI/Main.sh
    - chmod +x /builds/tklusz/childsafelegal/DevOps/CI/Deploy.sh
    - cd /builds/tklusz/childsafelegal/DevOps/CI/
    - ./Base.sh
    - ./Main.sh
    - ./Deploy.sh


```

The CI has been separated out into 3 different bash scripts. This is a major improvement for organization and updating. It's now possible for someone to work on the base image, while someone else works on the deploy image.

#### Inside the new bash scripts.
These scripts are still relatively simple, but there are a few important changes. 

First, the AWS-CLI is now being installed in the "base" container instead of the "main" container. This allows us to avoid running apt-get update again for the "main" container. In practice, this saves around 45 seconds of build time per operation. Also, this container will be stored in GitLab's Container Registry in future to improve build times (Removing around 2-3 minutes of build time).

Another important change is that all of the code has been given in-line commenting, which wasn't possible in the original .yml file. 

Base:

```
#!/bin/bash

# Moving production code to the Docker directory.
mv -v /builds/tklusz/childsafelegal/Dev/Build /builds/tklusz/childsafelegal/DevOps/Docker/Main

# Updating the container.
apt-get update

# Installing docker, python-pip, awscli
apt-get install -y docker python-pip
pip install awscli
apt install -y docker.io

# Starting docker
service docker start

# Moving to base dockerfile directory then building the base image.
cd /builds/tklusz/childsafelegal/DevOps/Docker/Base
docker build --rm=true -t base .

```
Main:

```
#!/bin/bash

# Moving to main dockerfile directory then building the main image.
cd /builds/tklusz/childsafelegal/DevOps/Docker/Main
docker build --rm=true -t build .

# Tagging our build with the commit SHA.
docker tag build:latest 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr:$CI_COMMIT_SHA

```

Deployment:

```
#!/bin/bash

# Logging into AWS region 2.
$(aws ecr get-login --no-include-email --region us-west-2)

# Pushing our image to ECR directory.
docker push 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr

```

#### Future Weeks
In the next two weeks, this functionality will be tested more and improved, and we will be using GitLab Container Registry to store artifacts from the Base image to reduce runtimes. This base image will only be ran when we want to update the base software.



--- 
title: "Tyler's Blog. Week 13." 
description: "Docker + GitLab CI" 
date: "2018-11-20" 
categories: [ 
    "Blog",
] 
menu: "main" 
tags: ["Tyler Kluszczynski", "HAT", "Week 13"]
---

#### What's new this week?
Docker and GitLab CI for basic CI/CD. This will be improved in the following weeks.

#### GitLab CI (.gitlab-ci.yml)
The gitlab yml file is fairly similar to the CircleCI one. In this case, instead of using the machine like in last project, I am using an ubuntu 18.04 LTS image. This is because our servers will be running 18.04, and it's ubuntu's LTS version, so it won't need to be updated as frequently.

```
image: ubuntu:18.04
```

We are only using one stage for now, the build stage.

```
stages:
  - build
```

Then we have our main build section:

```
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

This is fairly compact so I will break it down further here. 

First, we set this section to run only during the master branch. 

```
get_code:
  only:
    - master
```

Then, we set the name of the stage, in this case it's "build."

```
  stage: build
```

Then when the GitLab CI runs, it will automatically clone the repo. 

The first part of the build is moving the "build" directory on our repo (which will contain the website code) to the same directory as the first dockerfile. After this, we update then install python, docker, and python-pip in our docker container. After this, we install docer.io and start the docker service. This is all preparation for us to run our dockerfiles.

```
  script:
    - mv -v /builds/tklusz/childsafelegal/Dev/Build /builds/tklusz/childsafelegal/DevOps/Docker/Main
    - apt-get update
    - apt-get install -y docker python-pip
    - apt install -y docker.io
    - service docker start

```

Next, we move to our base dockerfile directory and run docker-build, naming the first image "base." After this, we run docker-build on our main docker image, calling it "build." This will execute all of the code in both of our dockerfiles. 

In future, the base image will only be ran when we want to update it (such as adding new software to the base image etc). I plan on storing the image resulting from the docker-build in Gitlab's registry, then having a separate stage for "only: update-base." This should save us around 2 minutes of build time.

```
    - cd /builds/tklusz/childsafelegal/DevOps/Docker/Base
    - docker build --rm=true -t base .
    - cd /builds/tklusz/childsafelegal/DevOps/Docker/Main
    - docker build --rm=true -t build .

```

Following this, we tag our docker image so it's AWS compatible, and we add the commit SHA.

```
    - docker tag build:latest 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr:$CI_COMMIT_SHA
```

Then, we install awscli, use aws ecr get-login to get login details for ECR, then docker push our new container into the ECR.

```
    - pip install awscli
    - $(aws ecr get-login --no-include-email --region us-west-2)
    - docker push 507963158957.dkr.ecr.us-west-2.amazonaws.com/ecr

```

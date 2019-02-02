---
title: "Erik's Blog. Week 17."
date: 2019-02-01T13:26:36-08:00
draft: false
tags: ["Erik Blomquist", "BEATS", "Week 17"]
layout: 'posts'
---

# CIT 481 - SAPS
#### Week 2
We have been focusing on the documentation this week too so that all of our members can be on the same page moving forward. I have looked over and documented our GitLab CI, ECR, Docker and Docker-compose which I learned a lot from, even that I did not write much of the code.

Here is an example of what our GitLab CI documentation looks like.
### GitLab CI - build
- The build stage uses the latest docker image (**image: docker:latest**).
- The first script uses **--no-cache** to disable cache locally. It also installs **curl**, **jq**, **python**, **py-pip** and **docker**.
- **pip install awscli** uses pip to install the AWS cli.
- **$(aws ecr get-login --no-include-email --region us-west-2)** is used to convert AWS credentials into a docker friendly format.
- **$REPOSITORY_URL** and **AWS credentials** are added as variables within GitLab, so that they don't get published accidentally.
- **docker build -t $REPOSITORY_URL .** is used to build our image from our repository url.
- **docker push $REPOSITORY_URL** is used to push the images into the repository.
- It only works with the master branch.


```
build:
  image: docker:latest
  stage: build
  script:
    - apk add --no-cache curl jq python py-pip docker
    - pip install awscli
    - $(aws ecr get-login --no-include-email --region us-west-2)
    - docker build -t $REPOSITORY_URL .
    - docker push $REPOSITORY_URL
  only:
    refs:
      - master
```

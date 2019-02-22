---
title: "Matabit Shahed Week 20"
date: 2019-02-21T09:40:08-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 20"]
layout: "posts"
---

# CI/CD

This week we've finally managed to finish our CI/CD Pipeline. After extensive research and help by our Professor, we've found the ideal and easiest way to deploy our containers from our GitLab environment.

## Deploying our Staging Environment
To deploy our staging environment, we use the following CI configuration:

```YML
deploy-staging:
  stage: deploy-staging
  image: docker:latest
  script:
    - apk add --no-cache curl jq python py-pip
    - pip install awscli
    - aws ecs update-service --cluster saps-staging-cluster --service app --force-new-deployment
  only:
    refs:
      - master
```

First, we are making sure that the docker image has all the necessary tools to use and install the AWS CLI, then we use the AWS CLI to update the service and force a new deployment, which ensures that we get the newest image that is pushed by our build-stage.


## Deploying our Production Environment
The same thing happens in our deploy-prod script:

```YML
deploy-prod:
  stage: deploy-prod
  image: docker:latest
  script:
    - apk add --no-cache curl jq python py-pip
    - pip install awscli
    - aws ecs update-service --cluster saps-prod-cluster --service app --force-new-deployment
  only:
    refs:
      - master
  when: manual
```

Here we add the `when: manual` directive, which allows us to manually approve the deployment process, when we are sure it is ready for the production environment.

# Monitoring

We are still in the process of finding our monitoring solution, as it is difficult to monitor FARGATE containers in AWS. CloudWatch offers multiple metrics, however we want to centralize all the metrics in a tool like Prometheus. However, we either have to decide to stick with CloudWatch for monitoring or export the CloudWatch data to something like prometheus to further analyze them.

---
title: "Matabit Anthony Week 17"
date: 2019-02-01T09:37:34-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 17"]
layout: 'posts'
---
# Week 17
This week was uneventful because I had to focus on other things this week. A bulk of the week was spent calculating the cost of switching over to an ECS infrastructure. Also had a small sprint/stand-up with the development team to get a good overview at where we're at. 

## Calculating ECS costs
I currently have a t.2 micro spot instance running the application which is dirt cheap. Less than ~20 cents a day. On the base level it looks like an ECS Fargate cluster will be slightly cheaper than my current spot instance, however, this doesn't account for an ALB to handle routing to the cluster. 

| Min config      | Min Config     | Optimal       |
|-------------    |------------    |-----------    |
| CPU             | 0.25vCPU       | 0.5vCPU       |
| RAM             | 0.5GB          | 1GB           |
| $ Per hour      | $0.01234       | $0.02469      |
| $ Per day       | $0.29622       | $0.59244      |
| $ Per month     | $8.8660        | $17.77320     |
| 2 Tasks         | $17.77320      | $35.54640     |

## CI/CD Pipeline with Laravel using Drone
I'm using drone as my CD/CD tool because I wanted to use something besides TravisCI, CircleCI, or Jenkins. In the first pipeline, the project is cloned and `composer install` runs to grab the php dependencies, including PHPUnit. PHPUnit is used to test the laravel app, which is the next stage in the pipeline. When the build and test pipeline successfully pass the deployment pipeline is called. This freshly clones the project, grabs the .env from an external location, builds the app in a docker container and pushes to an ECR. The continuous deployment isn't fully fleshed out yet, I still have to create a trigger for the ECS cluster to update when a new image is pushed. This will be done with Lambda. 

### Grabbing the .env file
Laravel requires a special .env file to function correctly. This holds secrets such as the database password, users, etc. This is always git ignored for security purposes. In order to grab the .env file during the deployment stage, I used the `mesosphere/aws-cli` as a stage to grab the .env file from an s3 bucket. I created an IAM use for drone tasks with limited permissions. The `mesosphere/aws-cli` container uses `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as environment variables that are stored in drone's secret's manager. 

### Drone YAML file
```yml
---
kind: pipeline
name: build-test

platform:
  os: linux
  arch: amd64

steps:
- name: build
  image: composer
  commands:
  - composer install --prefer-dist

- name: PHPUnit Test
  image: php:7.3-rc-cli-alpine
  commands:
  - cp .env.test .env
  - php artisan key:generate
  - vendor/bin/phpunit --configuration phpunit.xml

- name: slack
  image: plugins/slack
  when:
    status: [ failure ]
  settings:
    webhook:
      from_secret: slack_webhook
    channel: classrooms-drone
    template: >
      {{#success build.status}}
        Build {{build.number}} succeeded. Good job.
      {{else}}
        Build {{build.number}} failed. Fix me please.
      {{/success}}

trigger:
  event:
  - pull_request
---
kind: pipeline
name: deploy
steps:

- name: grab .env
  image: mesosphere/aws-cli
  environment:
     AWS_ACCESS_KEY_ID:
       from_secret: aws_access_key_id
     AWS_SECRET_ACCESS_KEY:
       from_secret: aws_secret_access_key
     AWS_DEFAULT_REGION: us-west-2
  commands:
    - aws s3 cp s3://matabit/test/.env.dev .env

- name: Deploy Image  
  image: plugins/ecr
  settings:
    region: us-west-2
    access_key: 
      from_secret: aws_access_key_id
    secret_key: 
      from_secret: aws_secret_access_key
    repo: 
      from_secret: repo
    registry: 
      from_secret: registry

trigger:
  status:
  - success

depends_on:
- build-test

trigger:
  event:
  - pull_request
  - push
...
```
---
title: "Matabit Shahed Week 19"
date: 2019-02-16T13:07:15-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 19"]
layout: "posts"
---

This week we are finishing up the CI/CD pipeline, implementing AutoScaling with Fargate, setting up a MySQL database server and creating our staging environment.

For us to finish the CI/CD pipeline, we need to introduce a new task definition, which will include the new frontend assets (built from angular). This will require us to write a Dockerfile specific for production, since we don't want to push an entire angular server, but rather just the compiled static assets.

We are going to try and do this through a multistage build, to be consistent with the asset compilation process.

Additionally, we need to update our `.gitlab-ci.yml` file so that we can introduce a deploy script, which will help us do our automatic deployment. This is going to be a shell script which will update the service to use the latest images.

So here are the steps, in which we do that:
- New Dockerfiles for frontend (production only)
- Update task-definition for multi container deployment
- Route traffic from ALB to nginx container (port 80)
- Update `.gitlab.yml` to include `deploy.sh` for automatic deployment 




--- 
title: "Tyler's Blog. Week 12." 
description: "Getting Started" 
date: "2018-11-15" 
categories: [ 
    "Blog",
] 
menu: "main" 
tags: ["Tyler Kluszczynski", "HAT", "Week 12"]
---

#### What's new this week?

This week, we started working on some of the backend Terraform to use for the project. We also discussed more details of the project with the CS team, attempting to lay more foundation about the technologies they would be using. They will be using the MEAN stack to build the application, which uses MongoDB, Express, Angular, Node, JS, NPM and Socket.IO. The other group members this week spent time creating the VPC, ECR and ECS for Docker containers to be uploaded to.

#### Researching GitLab-CI and basic functionality.

Our group was originally going to use Jenkis for CI, but the technology requires a server to be running to use. Instead, we decided to use Gitlab-CI for our CI/CD, as it is free and includes 2000 minutes of build/run time per month. If we exceed this amount, we will discuss with Crista about another option to use for CI. One option is setting up our own runner server for GitLab-CI, so we don't have to migrate the code. 

GitLab-CI has some similarities with CircleCI, but there are also many differences. In GitLab-CI, there are three main phases, the testing, building, and deploying phases. These are basically steps that the GitLab runner will run through. Much like in Circle, the CI code in the .yml file is ran in a container. We have decided that our instances will be Ubuntu 18.04 LTS, as this will have to be updated least frequently (as it's an LTS version, and supported by Ubuntu instead of a smaller Linux distribution). 

I created an AWS IAM account to use the GitLab-CI to upload onto AWS ECR. 

As of now, there is some test Docker code with Gitlab-CI on the repo, but this will be expanded more and tested in the following two weeks. It takes a very long time to run now (over 4 minutes), and was actually a requirement for the following 2 weeks. I am getting a head start on it now to hopefully have some free time during Thanksgiving next Thursday. More details about the Dockerfile and GitLab-CI code will be provided in next week's blog post once all of the kinks are worked out.

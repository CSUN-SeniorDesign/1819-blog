---
title: "Tyler's Blog. Week 26."
description: "New Project."
date: "2019-4-11"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 26"]
---

### What's new this week?
This week, we started planning for our new lightweight infrastructure. For our new infrastructure, we will be using Lightsail. This week, I created our design document, milestones, and basic network diagram. I also created a new GitLab repository, and integrated that repository with our Slack channel as well. Next week, we will start working on creating our new infrastructure.

This week was used to get everyone on the same page for the plan for our new infrastructure, and creating an initial design document and network diagram. The other members of my group spent this week finishing up tasks on our previous infrastructure. As of now, we have a high-level plan for our new infrastructure, and plan to get more granular as we discuss each service we will be using individually.

I created 10 milestones on our GitLab for various parts of our infrastructure on GitLab. These milestones can be referenced whenever we create a code commit or pull request, to showcase what we've been working on.

I also created our design document. This breaks down into:
* An overview section, which specifies the services we will be using.
* A context section, which specifies why we are creating the infrastructure.
* An existing solution section, which describes our current infrastructure, and how it is not sufficient for our plans.
* A goals section, which explains our primary goals for this new infrastructure.
* A non-goals section, which explains elements of our infrastructure which aren't a current priority (for example, 100% uptime).
* A milestones section, which states milestones we will achieve throughout the creation and implementation of our infrastructure.
* A service breakdown section, which breaks down each service and states what we will use it for.
* A resources and credits section, which states the resources we will use during the creation of our infrastructure. This list will be updated as our infrastructure progresses.

Lastly, I created our network diagram (which is relatively simple).
* We plan to use GitLab CI to:
    * Push Wordpress site code to an S3 bucket.
    * SSH into our Lightsail instance.
    * Upload a bash script to our Lightsail instance, ran as a cronjob.
* The bash script from GitLab should update the instance using code on our S3 bucket.
    * Using a CI-CD pipeline allows us to easily update the bash script if changes are required.
* We plan to use ACM for an SSL certificate, with DNS validation using Route 53.
* We plan on using Lightsail as our instance type, with a 20GB SSD (included in the $3.50 tier). This also includes a static IP.
* We plan on using Cloudflare as our Caching and CDN service. We may use it as our DNS service, if we can  automate it for our ACM certificate.

We expect our new infrastructure to cost around $4.50 a month with the current plan. We currently haven't decided how we are going to implement a backup solution.

#### Future plans
We plan to work on this infrastructure until it is complete. I heard a rumor that there will be a final project for the class (which will take multiple sprints to complete), so we may not finish the infrastructure completely if that is the case.

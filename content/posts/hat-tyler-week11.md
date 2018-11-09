--- 
title: "Tyler's Blog. Week 11." 
description: "New Teams" 
date: "2018-11-9" 
categories: [ 
    "Blog",
] 
menu: "main" 
tags: ["Tyler Kluszczynski", "HAT", "Week 11"]
---

#### What's new this week?

This week was the first week where we started working with our new group members. I am in the team doing childsafe legal. As of right now, we are still communicating with the CS students, the project owner and client to gather more information, but are also starting to build our infrastructure. For me, this week was mostly about coordinating the IS and CIT students as well as setting up our new GitLab repo, Slack, and integrating Slack with GitLab for ease-of-use.

#### GitLab Setup
I was primarly responsible for setting up the GitLab enviornment, so I will cover that this week, as S3 .tfstate synching and IAM has already been covered by me before. 

First, we invited all CS and IT students to a gitlab group for ChildsafeLegal. This was fairly straightforward after getting the Slack set up and meeting for the first time. 

Next, I made a contributions guideline for Terraform, and pull requests in general, so we can have a more organized setup for pull requests and Terraform code. This was a priority as my previous team (BEATS), had a very unorganized Terraform setup (with the .tf files). 

Then, set up issues with issue labels. Currently we have the following issues: "DevOps - Complete" "DevOps - Doing" "DevOps - To Do" "DevOps - Urgent", and the same issues for "Developers" instead of "DevOps". This allows us to prioritize and mark our issues.

Finally, I set up Slack integration with GitLab for much more conviennce. In previous projects, we had to @everyone on Slack or Discord to message other team members that our pull request needed a review to be pushed to master. Now, whenever an issue or pull request is posted on GitLab, a message is sent into the "Git" channel on Slack. 

With Slack integration, we can use /gitlab commands in the channel to show issues, move issues and create new issues. I also set up a Slack notification service using a Slack webhook for Issue and Merge request notifcations.

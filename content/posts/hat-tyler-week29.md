---
title: "Tyler's Blog. Week 29."
description: "EKS Progress."
date: "2019-5-2"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 28"]
---

### What's new this week?
This week, I created more AWS infrastructure using Terraform, including more modules and documentation.

#### My contributions
The following are the contributions I've made this week.

##### Housekeeping
* Organized our documentation and module directories.
* Added additional outputs to EKS master cluster, updated documentation.
* Updated `iam_role` module, added new 'path' input. Updated documentation  for this module.
* Imported `group_with_policy_and_users` module from childsafelegal GitLab and updated the documentation for new style.
* Updated setup instruction documentation, including information about the new Python script.

##### New Content
* Created `kube-update.py` script to update Kubernetes kubectl configuration and apply configuration map.


* Successfully connected to eks cluster with kubectl.
* Successfully connected eks nodes with the master cluster.


* Created `iam_user` module with documentation.
* Created new `policy`, `role`. `security group` and `instance profile` for worker nodes.
* Created `instance profile` module with documentation.
* Created `security group rule` module + documentation.
* Created `launch_configuration` module and documentation.
* Created `asg` module and documentation.

##### What I've Learned
* Using Terraform `template_file` (data) for json files (policies, roles etc).
* Kubectl basics, setting up Kubernetes using EKS and an ASG for nodes.
* It's possible to  specify a security group as a source for other security group rules, instead of a CIDR block.


#### Plans next week
Next week, I plan to set up a relatively simple Dockerfile to build an image for the Kubernetes nodes to run. We also need to create a presentation for Thursday, demonstrating the progress we've made on the project.

---
title: "Tyler's Blog. Week 28."
description: "Another new project."
date: "2019-4-25"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 28"]
---

### What's new this week?
This week I started working on our new infrastructure plans, after receiving the final project guidelines.

#### Infrastructure contributions
This week, I got our baseline infrastructure set up four our EKS deployment. The team is still researching and learning more about Kubernetes, but in the mean time, I set up some foundational infrastructure and documentation.

#### My contributions
This week, I created the new GitHub repository required for the new infrastructure, as well as a gitignore. I also created my own repository as a "fork" (even though it's not possible to fork your own repository). I created some documentation before creating infrastructure, including the design document, infrastructure diagram and setup instructions. These documents are a work in progress, and will be updated as our infrastructure changes.

Next, I created some Terraform modules for our new infrastructure, as well as documentation for those modules. I started by creating a module template, that can be filled out in order to make documentation for each module easier. After that, I started working on creating the modules themselves.

I created our VPC module, IAM role module, Role policy attachment module, EKS cluster module, and modified and imported our Security Group module from childsafelegal. The Security Group module now allows for blank egress or ingress rules. All of these modules are described in more detail in their relevant documentation section. Also in regards to documentation, each module is documented as well, and commented with block-comments within the file.

All together, I have around 600 LOC contributions this week.

#### Plans next sprint
Here are some plans I have for next sprint:
* Set up EKS using Terraform.
* Learn more about configuration of Kubernetes using kubectl.

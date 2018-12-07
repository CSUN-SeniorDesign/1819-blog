---
title: "Matabit Shahed Week 15"
date: 2018-12-05T19:10:42-08:00
draft: false
layout: "posts"
tags: ["Shahed Salehian", "Matabit", "Week 15"]
---

# Project Structure, CI/CD and Node.js

# Project Structure

Last week, after we received feedback from our professor on our decision to separate our project environments, we needed to make some adjustments to securly isolate them.

Our issue with the prior method was that even though we were using modules, we were hoping that things would properly get "copy and pasted" from one environment to another. However, with this method differences can creep in very easily. 

To fix this problem, we decided to create a modules folder that all the other environment will point to using modules as well. 

```bash
module "vpc" {
  source = "../../modules/vpc"
  
  vpc_name = "saps-vpc-prod"
  vpc_env = "prod"
  remote_bucket_name = "saps-prod-state-bucket"
}
```

As you can see in the codeblock above, with the source parameter we can pull in another module and use its resources and outputs as well. However, we have to explicitly output those variables.
Ideally, we would want to have the root modules in a separate repository to be able to version them, however that is not a priority and would potentially cause too much complexity once we hand off the project.


To visualize the current project structure, I have included the tree diagram below.

```bash
.
├── modules
│   ├── eip
│   │   ├── eip.tf
│   │   ├── output.tf
│   │   └── vars.tf
│   ├── remote_state
│   │   ├── output.tf
│   │   ├── s3.tf
│   │   └── vars.tf
│   └── vpc
│       ├── output.tf
│       ├── vars.tf
│       └── vpc.tf
└── prod
    ├── eip
    │   ├── eip.tf
    │   └── output.tf
    ├── remote_state
    │   ├── remote_state.tf
    │   ├── terraform.tfstate
    │   └── terraform.tfstate.backup
    └── vpc
        └── vpc.tf
```

# CI/CD

We struggled this week choosing the correct CI/CD tool, as we wanted to keep things as cheap as possible and we didn't want to put any strain on our budget. Additionally, we we wanted to ensure that we didn't have to move where our repository is hosted. Our constraint in that matter is that we need a private repository and they cost money. AWS CodeCommit allows for 5 users free and charges $1/month for every additional user.

Using CodeCommit would have been ideal, as it would perfectly integrate with the AWS CodeStar tools to easily create our CI/CD pipeline.

GitHub only offers free private repositories to students, and that would make it more difficult to handoff to the larger CSUN SAPS organization.

We ended up settling on GitLab as it allows for free private repositories with unlimited users and provides CI/CD tools with up to 2000 build minutes every month.

Initially, we were hoping to use the AWS CodeStar tools with our repository being hosted on GitLab. However, AWS doesn't support GitLab repositories in their tools.


# Node.js

Speaking to the Computer Science team we have found out that they are rewriting the application in Node.js instead of .NET Core. The stack that they are going to be using is Node.js, React.js, Express.js and MySQL. We have no bias towards any of those technologies as we have to learn to accomodate any technology that is going to be given to us, be it Ruby on Rails, .NET Core or Node.js









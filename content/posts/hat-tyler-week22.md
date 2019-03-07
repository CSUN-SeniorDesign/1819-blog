---
title: "Tyler's Blog. Week 22."
description: "Datadog + ECS."
date: "2019-2-28"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 22"]
---

### What's new this week?
This week I've been tasked to help Hayden with setting up our monitoring solution.

Hayden worked on monitoring last week, but only set up monitoring using the AWS console, opposed to Terraform. He also had some issues getting it running on his local machine earlier into the week. Because I finished the RDS early, I had extra time to set up our monitoring using Terraform.

For our monitoring solution, We chose to use Datadog for a couple reasons. We already had some limited exposure to it, it was already included in our GitHub student account (for free), and it was fairly easy to set up and integrate with AWS.

#### Datadog setup
I set up the Terraform required for our Datadog monitoring.

* First, I created a module for task_definitions, located in /src/task_definition. This allows a user to create a task definition using a json file as the container_definition input. It could use more modularization, as the volumes are statically assigned to the ones required for Datadog currently. The json files for task definitions are located in TerraformUsingModules/task-definitions/.

* The module also creates a service using the task definition. In this case, our scheduling strategy is DAEMON. This will place an instance of our container in every AWS container instance currently running. This is useful for monitoring, as we want to monitor each container instance.

* Next, I created a Terraform file for Datadog, that uses our new module. It's located in TerraformUsingModules/datadog.tf. It utilizes the module to create a new task definition for our Datadog, along with the service. After the service and task definition are created, Datadog will start receiving system metrics.

* At this point, system metrics are set up and working along with the infrastructure set up in Terraform. In future sprints, I would like to add more monitoring (log + trace collection) for more experience.

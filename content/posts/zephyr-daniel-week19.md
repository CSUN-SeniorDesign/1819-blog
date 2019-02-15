---
title: "Irrigation Post - Week 19"
date: 2019-02-15T11:03:18-08:00
draft: false
tags: ["Daniel", "Zephyr", "Week 19"]
---

# Week 19
Week 19 I was tasked with integrating Datadog monitoring software into our server. I had to refresh myself with how to do the task as originally I only know how to do it using the console on AWS. Since we want it directly on our private instance right now I have to install an Agent directly onto our instance. We are using Ubuntu as our OS and datadog has some easy documentation to follow when it comes to integrating an Agent on Ubuntu. This involves typing in this command into the command line in ubuntu.

      DD_API_KEY=123456 bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh)"
This command will install the packages needed for the agent and will then prompt me for the password to activate the agent. If I have any problems I can always install it using a long way by setting up a repo on the machine and downloading the files needed to install datadog into the repo manually.

Another task I need to finish is implementing the pipeline into our network. We were using Laravel before but the Irrigation Software team has updated their site. Now I have to update the pipeline to still be able to deploy a Docker Container but instead build a site using Javascript, Angular and Node.js.


# Tasks as of Now
After installing the Agent I have to configure it in the command line by editing the configuration file. After doing so and getting the correct analytics we want from the server and seeing if it displays correctly on the Datadog site I can continue working on the GitLab. Working on the Gitlab would be to still try to get the pipeline up so it can correctly build the Docker Container and build the site correctly using the dependencies it needs.

---
title: "Brian's Blog. Week 12"
date: 2018-11-16T14:22:02-02:00
draft: true
---

### This week's overview
After being able to talk about the layout of the infrastructure with Leo we managed to get a much clearer overview of what is going on and how things are being ran. We found out that everything is being ran on one bastion host which is a bit risky because data that is on the host can be lost if anything were to happen to the host. There are various databases that are being ran as well we have noticed that the current setup has an s3 bucket in place and everything that was set up was being done through the console interface on AWS. Our current rough objective is to provide automation to this infrastructure so that in case of any disaster we can recover and set things up quickly. We will be containerizing everything through docker and include more features like a load balancer and the use of ECS along with other features. Some of the issues that have arise when trying to figure out some of the things is that tasks and source code are being kept secured using secrets manager so their is a learning aspect to figure that out as well and seeing how we can integrate secrets manager to work efficiently with our infrastructure. We are going in blind so to say since there is as well no documentation or notes of what has been done.

By the end of the week we should have a diagram up which we will present next week to Leo and the rest of the teams so that they have an idea of what we are trying to do and that way we can get feed back on what we might be missing or there might be certain features they want included. In future long plan terms we want to create a system that is easily moveable and we can leave detailed documents to help the people that will take over the project after we have left.

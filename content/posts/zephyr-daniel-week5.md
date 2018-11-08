---
title: "Project 2 Post 1: Daniel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 5"]
date: 2018-09-28T14:33:34-07:00
draft: false
---

For this project I was tasked with doing AWS Auto Scaling with HashiCorp Packer. After being assigned these tasked I went to AWS console to see what they had in regards to Auto Scaling. I found that in the EC2 instances that they had a built in Auto Scaling and so I read some documentation about it and learned that it's as easy as setting up an instance. 

Reading the documentation lead me to the Auto Scaling wizard where I had to setup Launch Configuration, then chose an AMI and the type of instance I would need. I chose the Free tier to prevent bills from being to high if it were to charge us. The documentation told me to setup a key pair, I thought it would be wise to use the one we already have because our terraform is setup with one .pem file that gives access to what it needs. If that was the case for that then using the existing key to use for the auto scaling made sense. This was just to create the launch configuration, afterwards I had to create Auto Scaling Group. 

For the group I had to select what name I wanted to call it, which I just called it something that I can identify it easily like Project2-ASG. Then set the group size which effected the amount of instances it would create, the project requirements doesn't really give much details about how many so as a group we decided to create a max of 2 for now and a minimum of 1. The network section was a little confusing because it needed a VPC but not one we created but one that was "default" Amazon. This type of VPC didn't exist in our AWS so I had it create a new one. After creating and selecting this VPC I had to give it subnets for it to use, I wasn't quite sure if the Auto scaling needed to use public or private and after talking to my group we decided that it was appropriate for it to use only the private subnets. 

After reviewing the Auto Scaling Group I created the group and then verified that it was working. To verify that it worked I had to see if it had created the instances after finishing it, which looked like it didn't. The Activity History tab created a bunch of errors saying it didn't have permissions to do so which I assumed meant they weren't able to create the instances. I don't have a screenshot of the error but I didn't bother to check the EC2 tab to see if it actually didn't create the instance but it did. There were new instances that were created that using the ASG which was strange. 

Unfortunately I realized that instead of using the ASG through AWS that I had to create the ASG on terraform as well as use HashiCorp Packer. Both were an oversight on my part but it's something I'll be able to try to know how to do over the weekend.

One of the Documentation that I had used for guidance
  https://docs.aws.amazon.com/autoscaling/ec2/userguide/GettingStartedTutorial.html

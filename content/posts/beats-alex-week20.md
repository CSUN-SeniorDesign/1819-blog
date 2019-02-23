---
title: "Alex's Blog Week 20."
date: 2019-02-15T11:23:49-08:00
draft: false;
tags: ["Alex Enaceanu", "BEATS", "Week 20"]
layout: 'posts'
---

# CIT 481 - SAPS
#### Week 20
This week was relatively slow. Not as slow as last week (which is why I had nothing to blog). All we had to do was create a staging environment in AWS. Luckily, all we had to do was duplicate the Terraform code we had written for the production environment. After that was done I went through the code and replaced the word "prod" with "staging". I ensured that all the replacements were made and I also changed the line of code that in our ECS module that creates 2 containers to 1,  since we don't need 2 in the staging environment:

      desired_count       = 1


 As you can imagine, there were quite a few changes to be made since we have to create new buckets, ALB, ECS clusters, etc.  After the code was altered I went ahead and ran each module (we use Terraform modules) in a specific order since some resources need to be created before others. Here is the order I used to create the environment:

     remote state-->eip-->vpc-->s3-->ecs cluster-->ecr-->alb-->ecs

The first time around we had some errors that were very minor and we were able to resolve quickly. the second time around the entire environment was created without issue! I logged into the AWS console and verified all the new resources appeared.

It was nice to work with Terraform again since I haven't touched it since last semester and also since I had to set it up from scratch on a new computer. All in all this weeks task were not very difficult but time consuming. I am excited to see hopw far we have come and am hoping that the CS team can get some code written so that we can start testing the actual SAPS and hopefully see it deployed before we graduate!

---
title: "Brian's Blog. Week 14"
date: 2018-11-30T14:22:02-02:00
draft: true
---
# Week 14 Sandoval

### This week's rundown
This week I was able to finish the s3 bucket with remote stating and as well started on creating the vpc for our infrastructure. I was initially scripting the VPC the way that i had installed it for the previous project with my group but my team member Shahed found a different way about going about it. It was recommended that I use a module instead of having to write everything up from scratch by doing it this way I am able to create the subnets easier and all at once as opposed to the latter. One of the issues that I am currently working on is that the subnets are created but not all of them. Another issue that arise is that of the remote elastic ip lock the issue that I had was that I couldnt recognize the elastic ip to the remote state file in my bucket in order to finish this up I ended up having to configure the data function hat will input this information into the remote state file within our bucket,

An issue that came up after presenting in class is that we initially made our design to include a staging, production, and development type but we were just going to copy the code for our production site to the staging and development type. Professor Hamilton said not to go with this idea of doing tings because we may run into errors when working on scripts and when we try to make changes to the other two stages we will be running into errors since it is harder to control and keep track of all the changes that we made. So our plan now is to still use modules in a way where we can create variables to be able to access only the stages that we need at the moment to configure it.

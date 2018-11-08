---
title: "Project3p1 Hayden"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Hayden Wilcox", "Zephyr", "Week 7"]
date: 2018-10-12T21:02:22-07:00
draft: false
---

Task Assigned this Week:
	- Research and setup Amazon Elastic Container Service (ECS)

Work Done:
	I reasearched what exactly ECS is and what it does. After watching the introductory video and reading through the documentation, 
I felt like I had a decent grasp of the concept. From there I was a bit confused where to proceed, as there was nothing on the introductory page 
that described exactly what needed to be set up. I soon realized that the first step in readying the container was to create an autoscaling group (ASG).
With this knowledge I began to look into how to create one, as it was not one of my responsibilities last project. From here, I copied the files
that my team had made as a base. I found a document that detailed what needed to be set up in order to get an ECS up and running. After creating some 
new files and editing currently existing files, I attempted to run terraform init, to begin the process of bringing it online. However, at this moment
an error appeared:

Error configuring the backend "s3": No valid credential sources found for AWS Provider.

	I figured this message had to do with the AWS access keys, and asked my group members how to find them. At this point we realized that I did not have
access to a number of things on AWS, among them being the access keys, but also specific files such as the S3 bucket. This proved to hamper my ability
to continue with the project for a number of days. I was able to get ahold of the s3bucket.tf file, but this did not solve the issue.

Work to be Done:
	Over the course of this week, it became apparent that even though I had previously had to set up and work with a terraform file, there was still much 
I did not know about terraform files and what many of the options mean. Therefore, my main goal for this weekend is to read through all documentation
regarding Terraform from the previous two projects, to be able to setup a single instance, then an ASG, then finally work back into being able to setup
an ECS. At this moment in time, I still do not have access to an access key or the group corporation's S3 bucket, which will require me to work with my
group members to rectify my access permissions, or to figure out a way of getting the necessary components to me.

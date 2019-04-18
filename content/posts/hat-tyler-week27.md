---
title: "Tyler's Blog. Week 27."
description: "Project Progress."
date: "2019-4-19"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 27"]
---

### What's new this week?
This week, I have made a lot of progress on our infrastructure. Some plans from the original design document have changed, so I will cover those first, then dive into what I worked on.

#### Infrastructure changes
Some noteworthy changes to our infrastructure changes:
* We are using Prometheus instead of ELK, due to system resource problems (this will be covered in more detail in next week's presentation).
* We no longer require use of a CI-CD pipeline, as there is a user-data section in the Lightsail instance that allows us to input a script that's run when the instance is first created.

#### My contributions
My main contribution this week was setting up our Lightsail module, including a page of documentation for each part of the module. The module outputs a key that can be used to SSH into the instance, and the IP address of the instance. The module also makes it easy to change the instance size and AWS region. I also set up a static IP for the Lightsail instance, and created documentation for each step required to create and delete the infrastructure. 

I also attempted to encrypt the private key using a PGP key, but it would require too much effort on the part of the end-user, and would complicate the infrastructure unnecessarily. Some concerns with not using a PGP key are that the private key is stored in the .tfstate file, so the end-user has to take care to keep both the private key and state file secure. 

In order to combat this, I added instructions for how to manually delete all of the AWS infrastructure, if a user wants to delete their .tfstate and .terraform files after the infrastructure is set up. This isn't a major issue, as our infrastructure is planned to be relatively simple, so the user should only have a few resources to delete manually.  

Later in the week, I attempted to set up ElasticSearch, which was unsuccessful due to system resource problems. Then, I successfully set up Prometheus and node-exporter using the user-data script. 

Some more minor contributions were setting up the gitignore file to ignore .terraform and .tfstate files and the instance's private key if a user accidentally forgets to delete it. I also created the provider.tf which sets the region to "us-west-2". I also created our infrastructure diagram.

#### Plans next sprint
Here are some plans I have for next sprint:
* Create documentation for our user-data script once it's complete.
* Have a separate script that gets ran each time the instance is started up (as of now, if the instance restarts, Prometheus will stop).
* Use AWS CLI for Lightsail snapshots, potentially set up an IAM user or role for the Lightsail instance that allows creation of backups, using a script on a cronjob.
* Set up Grafana for Prometheus if I have enough time.

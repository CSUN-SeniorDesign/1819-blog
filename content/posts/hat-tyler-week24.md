---
title: "Tyler's Blog. Week 24."
description: "Elasticache Part 1."
date: "2019-3-28"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 24"]
---

### What's new this week?
This week I worked on setting up Elasticache with Memcached. This is a service on AWS that allows for caching of data in memory on an EC2 instance, that can be accessed from other instances. This is beneficial because there doesn't have to be any read/write to disks which takes up additional time.

For our current infrastructure, this isn't completely necessary because we aren't concerned about performance to that degree. This solution is suited to sites that have a large number of requests and data that's being sent to users. Even though this is the case, it's beneficial to learn how to set up Elasticache with Memcached for future reference.

#### Terraform changes
This week, I created a new module for Elasticache instances. This is located in /src/elasticache/main.tf. The module is already commented with block comments, and creates an Elasticache cluster using various variables. It's also set to use multiple availability zones, and 2 instances (as we are using 2 availability zones, we will have 1 instance in each). The instances we are using (cache.t2.micro) only have .5GB of memory, which means we can only cache around 500MB of data.

Note that since these instances aren't regular t2.micro instances, they aren't covered by the free tier plan, and you will be billed regardless if you have free t2.micro hours on your monthly allowance.

Alongside the new module, I consolidated our security groups into 1 file, added the Elasticache port to our Wordpress security group, and created a security group for our Elasticache instances.
Elasticache also requires a elasticache-subnet-group, which I created in our VPC and use in our Elasticache module.

#### Future plans
The Elasticache isn't currently connected to our Wordpress container instances. This is because our storage is being changed from EFS to S3 by Aubrey as per request from our professor, which may change how data is stored and how plugins are created.

I also require a plugin on our Wordpress instances in order to connect to the Elasticache with Memcached. Because of this, I plan on waiting until the data migration is complete before setting up the plugin. I'm also planning on getting additional information on how our Wordpress Dockerfile is set up (by Aubrey) in order to add the new plugin using the Dockerfile.

I plan on having the connection complete before the presentation on the 8th of April.

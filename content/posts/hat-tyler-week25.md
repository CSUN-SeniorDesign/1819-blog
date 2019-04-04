---
title: "Tyler's Blog. Week 25."
description: "Elasticache Part 2."
date: "2019-4-05"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 25"]
---

### What's new this week?
This week, we completed the setup of Elasticache, and connected it to our Wordpress instances. I updated our security groups to allow the connection between Elasticache and Wordpress instances.

 I also updated our CI-CD Pipeline to utilize the "aws ecs update service" command, which will automatically update the ECS service whenever we push a new image. This requires another permission added to our GitLab bot account, which I also added.

#### Challenges this week.
There were numerous challenges that our group ran into this week. One that I experienced was connecting our Elasticache instances to our Wordpress ECS instances. In order to successfully connect both instances, It took around 8 hours of work.

To begin with, we connect to the Wordpress instance via the load balancer, then log into the admin console. In the "plugins" section, navigate to the w3-total-cache plugin. From there, we can navigate to our database caching, then enter a memcached hostname. When entering the hostname, there's an option to test the connection, which failed.

Research into why the connection failed, we discovered we need to install not only the plugin, but a AWS memcached extension for PHP. Tuesday this week, I attempted to set up the extension using our Dockerfile, but there were numerous challenges. First, the memcached extension wasn't compatible with the present version of PHP (which we were using). Also, I didn't fully understand how our Wordpress dockerfile was configured.

As a result, I moved on to setting up "aws ecs update service" with our CI-CD pipeline, and asked Aubrey to help installing the PHP extension. Aubrey managed to install the plugin on Wednesday night, and on Thursday I updated our security groups, and tested the connection AWS, which worked.

#### Future plans
The Elasticache isn't currently connected to our Wordpress container instances. This is because our storage is being changed from EFS to S3 by Aubrey as per request from our professor, which may change how data is stored and how plugins are created.

I also require a plugin on our Wordpress instances in order to connect to the Elasticache with Memcached. Because of this, I plan on waiting until the data migration is complete before setting up the plugin. I'm also planning on getting additional information on how our Wordpress Dockerfile is set up (by Aubrey) in order to add the new plugin using the Dockerfile.

I plan on having the connection complete before the presentation on the 8th of April.

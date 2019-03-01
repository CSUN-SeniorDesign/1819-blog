---
title: "Tyler's Blog. Week 21."
description: "RDS with ECS"
date: "2019-2-28"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 21"]
---

### What's new this week?
This week, I've been tasked with setting up an AWS RDS to work with our ECS Wordpress container. This involves creating a new Terraform module, and learning about how AWS RDS works.

### RDS
AWS RDS is a relational database service offered by AWS. RDS uses a "database instance class" which is basically a EC2 instance specialized for use in RDS.

* The first problem I ran into this week, was attempting to set up Aruroa on RDS. For an Aurora RDS instance, we would require at least a db.t2.small instance (and we are planning on running 2 instances), which was above how much we wanted to spend. As an alternative, we decided to use the MySQL engine, allowing us to use a db.t2.micro instance instead.

* Another challenge I had this week was setting up the RDS database subnets. RDS requires a specific subnet group called an aws_db_subnet_group, which is a collection of all subnets the database instances could be placed in upon creation. For our project, we are using our private subnets as the db_subnet_group. I also attached a security group to our RDS, the same one we were using for our database container last week.

* Our RDS is using a replica as part of the infrastructure as well. Eventually we plan to have 2 ECS Wordpress instances, one connected to the master database, and the other connected to the replica. I also enabled Multi-AZ for our RDS instances, allowing one instance to be placed in us-west-2a and the other to be placed in us-west-2b.

* Near the end of this week, I finished setting up our RDS and successfully connected our RDS instance to our ECS Wordpress instance.

### Next week
Next week I plan to help Hayden with Datadog, as I finished RDS ahead of schedule.

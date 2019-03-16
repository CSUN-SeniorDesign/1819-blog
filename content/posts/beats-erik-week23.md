---
title: "Erik's Blog Week 23."
date: 2019-03-15T10:37:47-07:00
draft: false
layout: 'posts'
tags: ["Erik Blomquist", "BEATS", "Week 23"]
---

## Auto Scaling Group for EC2 instances
We recently switched from Fargate to EC2 so we now need an auto scaling group that scales our instances automatically. The auto scaling group will perform periodic health checks and keep track of our desired count of instances and it will automatically replace one of them if they would become faulty or unhealthy.

### Launch template
We need to create a launch configuration/template that our EC2 instances will use to scale new instances. We can do that with the following:

- **name_prefix** - Name of your template.
- **image_id** - AMI Image. Make sure that the image is compatible with your region (https://aws.amazon.com/amazon-linux-ami/).
- **instance_type** - Which type of instance you want to create. t2.micro is used because it uses the AWS Free Tier.

        resource "aws_launch_template" "asg-launch-temp" {
        name_prefix   = "asg-launch-temp"
        image_id      = "ami-a0cfeed8" # Amazon Linux AMI for us-west-2
        instance_type = "t2.micro"
        }

### Auto Scaling Group
Next step is to add the Auto Scaling Group itself. 

- **availability_zones** - Which AZ to use.
- **desired_capacity** - Prefered number of instances.
- **max_size** - Maximum number of instances allowed.
- **min_size** - Minimum number of instances allowed.

- **id** - Link to launch template.
- **version** - Get the latest version (Available commands are **latest** and **default**).

        resource "aws_autoscaling_group" "asg-test" {
            availability_zones = ["us-west-2a"]
            desired_capacity   = 2
            max_size           = 4
            min_size           = 1

        launch_template {
            id      = "${aws_launch_template.asg-launch-temp.id}"
            version = "$$Latest"
        }
        }


Run terraform apply and wait for the changes to be applied. You can increase the number of desired_capacity and run terraform apply again and you will see new instances starting up. Congratulations! :)

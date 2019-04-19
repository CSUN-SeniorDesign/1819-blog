---
title: "Irrigation Post - Week 27"
date: 2019-04-19T14:41:18-07:00
draft: false
tags: ["Daniel", "Zephyr" , "Week 27"]
---

# Week 27
This week I continue my endeavor on working on the Terraform. Closely following our documentation to understand how everything is done and give me an insight why it might be done this way.

To give some insight on how I started I'll start with the provider.tf file.

```
# Configure the AWS Provider
provider "aws" {
  region     = "us-west-2"
  access_key = "anaccesskey"
  secret_key = "asecretkey"
}
```
This is the skeleton that the Terraform documentation provides when creating this file. Which I thought was the only way, the other way being having them as variables and then referenced. Both required Terraform files where you have your secret and access key located in a file. One thing I learned was This doesn't have to be the case with the combination of the aws credentials file. AWS CLI lets you configure your profile and will store your aws credentials locally which is found in
```
.aws/credentials
```
When creating the provider file you can just reference this file by declaring the path to this file. In our case if we didn't provide anymore than just the provider resource terraform knew to look there by default.

The next step was to create the VPC. One thing I also didn't consider was to create everything modular in terraform. The first thing I did when creating these files was just bunch them in a huge file where as I should have separated them so that I know what each file does and if something goes wrong it's easier to track the issue. SO the next file I needed to create was the vpc.tf file.

```
resource "aws_vpc" "irrigation-vpc" {
					  cidr_block   	= "172.31.0.0/16"
					  instance_tenancy = "default"

					  tags {
						Name = "irrigation-vpc"
					  }
					}
```

Following the documentation on terraform and what we have on our repo this is a typical skeleton of what we need in a VPC file. We just needed to replace the cidr_block with what we needed. The instance tenancy is set to default and we added a tag to easily reference it when we are in the console. I tried to look around to what the tenancy does in this case. I couldn't really find much information about it other than it really defining itself as how the instance is spun up. So I left it as default.

The next file that I added was the gateway.tf which was our internet gateway resource. This file enables our VPC to communicate with the internet, the file itself is really simple and only requires around 4 lines of code.

```
resource "aws_internet_gateway" "irrigation-gateway" {
					vpc_id = "${aws_vpc.irrigation-vpc.id}"

					tags {
					Name = "irrigation-gateway"
					}
			}
```

We assign it a vpc so it knows which one to give it internet access and then we tag it with something we can reference too whenever we are in console.

# Tasks as of now

I continue to finish up the rest of the Terraform. This was only a small snippet of what I was working on this week. The rest of the tasks include working on ALB, IAM roles, EC2 and setting up an s3 bucket. These are all things that I'm trying to setup using terraform for our aws infrastructure learning from our own Documentation and the Terraform Documentation.

---
title: "Matabit Shahed Week 14"
date: 2018-11-28T17:25:09-08:00
draft: false
layout: "posts"
---

# Project Layout, VPC, and Terraform Modules

After some thorough research, we have finally found out the best practice in terms of building the project layout.

The author of "Terraform Up And Running" Yevgeniy Birkman, advocates for a complete separation of environments. 


## The Structure

- terraform
	- global
		- iam
		- route53
	- prod
		- vpc
		- eip
		- alb
		- remote_state
		- [...]
	- staging
		- vpc
		- eip
		- alb
		- remote_state
		- [...]
	- dev
		- vpc
		- eip
		- alb
		- remote_state
		- [...]


## Reasoning

This way, if any of the environments have to change, it would not affect anything else by accident. We want each environment to be contained in a separate VPC and have it's own infrastructure. This is more expensive than having to throw everything into one VPC, however, it is the safer and more secure alternative. Production ideally has to be completely separated. 

## Current infrastrucutre

Currently, we are only developing for the production environment. If we can establish one environment successfully, we can simply copy the same configuration for the development and staging, to ensure equality between the environments.

As of now, we have our environment setup with two private subnets, and two public subnets, an internet gateway and a NAT gateway sitting in a public subnet and the private subnets routing all outbound traffic through the NAT gateway.

## Modules

Yevgeniy Birkman also advocates for using Modules in terraform code, since those modules have been tested and a lot of companies have the same needs in terms of infrastructure, so there is no need to reinvent the wheel everytime.

This is why we ended up using a VPC module from the module registry offered by terraform.

Here is how we set that up: 

```
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"

  name = "csun-saps-vpc-prod"
  cidr = "10.0.0.0/16"

  azs             = ["us-west-2a", "us-west-2c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]

  enable_nat_gateway      = true
  enable_vpn_gateway      = false
  single_nat_gateway      = true
  one_nat_gateway_per_az  = false
  reuse_nat_ips           = true
  external_nat_ip_ids     = ["${data.terraform_remote_state.eip.nat-eip-id}"]

  tags = {
    Terraform = "true"
    Environment = "prod"
  }
}
```






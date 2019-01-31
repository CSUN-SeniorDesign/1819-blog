---
title: "Tyler's Blog. Week 17."
description: "Terraform Modules"
date: "2019-1-31"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 17"]
---

#### What's new this week?
This week has been primarily focused on contributing to Terraform modules.
Terraform modules are basically a way to translate Terraform code into a reusable "function" that can be "called" and will create specific resources based on what is declared in the module. For this week, I will show an example of one of my new module files.

#### Terraform Module Example.
Our code is hosted on GitLab, as we assumed the client would use AWS and would want to keep the code private. Since the client has decided to use a different hosting platform (instead of AWS), I don't mind sharing the code now.

For this example, I'll be showcasing the /src/vpc_ig_subnets/main.tf, which allows for creation of a VPC with an internet gateway and subnets.

vpc.tf:

```
# Creating VPC with Internet Gateway and subnets.
module "vpc_and_ig" {
  source = "./src/vpc_ig_subnets"
  cidr_block = "172.31.0.0/16"

  public_subnet_cidrs = ["172.31.1.0/26", "172.31.1.64/26"]
  public_subnet_azs = ["us-west-2a", "us-west-2b"]

  private_subnet_cidrs = ["172.31.0.0/26", "172.31.0.64/26"]
  private_subnet_azs = ["us-west-2a","us-west-2b"]
}

```

For the vpc.tf, we specify the source of the module, along with several variables the module uses to create the resources. In thsi case, we specify the subnet cidrs, cidr block, and availabity zone.

The /vpc_ig_subnets/main.tf is what actually creates our vpc and subnets. At the top of the file is a list of variables which are used whenever a new instance of the module is created (such as in vpc.tf).

/vpc_ig_subnets/main.tf - variables:

```
# Variables used in the creation of our VPC
variable "cidr_block" {}
variable "instance_tenacity" {
  default = "default"
}

variable "private_subnet_cidrs" {
  type = "list"
  default = []
}
variable "private_subnet_azs"{
  type = "list"
  default = []
}

variable "public_subnet_cidrs"{
  type = "list"
  default = []
}
variable "public_subnet_azs"{
  type = "list"
  default = []
}
```

After this, we use the variables declared to create our resources, instead of hard-coding the values of the resources. Note that the code is commented to add clarity. When you want to use a variable inside your module, use the syntax "${var.<variable_name>}".


/vpc_ig_subnets/main.tf - creation:

```
# Creating the VPC.
resource "aws_vpc" "autogen_vpc" {
  cidr_block = "${var.cidr_block}"
  instance_tenancy = "${var.instance_tenacity}"
}

# Creating the Internet Gateway.
resource "aws_internet_gateway" "autogen_gateway" {
  vpc_id = "${aws_vpc.autogen_vpc.id}"

}

# Generates our private subnets.
resource "aws_subnet" "private" {

  /*
    Count tells Terraform to repeat this creation a number of times.
    Using a length function to determine the length of our CIDR list.
    We are creating 1 subnet per item in the CIDR list, so this is how many times
    We repeat.
  */
  count = "${length(var.private_subnet_cidrs)}"

  # Using the VPC we created eariler.
  vpc_id = "${aws_vpc.autogen_vpc.id}"

  # This is the CIDR given as a variable, getting whichever CIDR is at the
  # current index.
  cidr_block = "${var.private_subnet_cidrs[count.index]}"

  # Availiability zones are also supplied as a list, using the same process as
  # cidr_block to obtain each value in the list as we loop through creation.
  availability_zone = "${var.private_subnet_azs[count.index]}"

  # Also tagging our subnets using the current index.
  # If we have 2 subnets they will be tagged "Private_0" & "Private_1"
  tags {
    Name = "Private_${count.index}"
  }
}

# Generates our public subnets, nearly identical code to aws_subnets.private.
resource "aws_subnet" "public" {
  count = "${length(var.public_subnet_cidrs)}"
  vpc_id = "${aws_vpc.autogen_vpc.id}"
  cidr_block = "${var.public_subnet_cidrs[count.index]}"
  availability_zone = "${var.public_subnet_azs[count.index]}"

  tags {
    Name = "Public_${count.index}"
  }

```

The most important part of modules is that the module can be called multiple times, to create multiple instances of the module.

Instead of having to write all of the code in /vpc_ig_subnets/main.tf each time you want to create a subnet, you simply fill in the variables (like what we see in vpc.tf). This saves a lot of time and space compared to not using modules.

This is just one example of the many modules I have created this week.

#### Next week's plans:
Next week I plan to finish the conversion of Terraform code into modules, and if I have more time, add more commenting to our other module files for better readability. All of the new modules are commented, but some aren't as well commented as the one in this example.

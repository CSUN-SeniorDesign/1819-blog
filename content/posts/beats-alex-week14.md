title: "Beats Alex Week14"
date: 2018-11-16T22:54:20-08:00
tags: ["Alexander Enaceanu", "BEATS", "Week 14"]
draft: true

## Week 14 - infrastructure is born
In week 14 we started doing more technical work since our many meetings have given us purpose motivation and direction. We will be using Terraform for our infrastructure code. We will do things a little differently by employing Terraform modules. Modules are pretty robust and there are many already built found on Terraforms module registry. Here are some key points regarding modules:
* Modules are essentially Terraforms equivalent to functions
* Modules work by pointing to and running Terraform code in a specific folder
* Can be comprised of many submodules that do specific tasks
* Highly configurable and customizable
* Entire infrastructure could be a module
* Modules can be shared, see The Terraform Module Registry
* Shared modules allow you to start with working code

We will be using pre built modules from the registry which saved us time since we didn't have to write all the underlying code that the module utilizes. Still the modules are highly customizable so we can alter them to our needs on the fly. Below are some examples of pre built modules we will use:

    output "vpc_id" {
    value = "${module.vpc.vpc_id}"
    }

    output "public_subnets" {
    value = "${module.vpc.public_subnets}"
    }

    output "private_subnets" {
    value = "${module.vpc.private_subnets}"
    }

This modules will execute the code that they are configured with, thereby outputting our VPC along with its public and private subnets. The details are contained within the modules themselves and all you need to create a simple or complex network scheme or even an your entire infrastructure can be preconfigured and called via modules. We think this method is easier and cleaner than writing our own code entirely. It also should make more sense for any newcomers that may be onboarded. This is precisely why clean and concise documentation will be crucial for proper continuity.

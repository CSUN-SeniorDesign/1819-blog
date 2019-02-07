---
title: "Tyler's Blog. Week 18."
description: "Finishing modules, Presentation and Documentation."
date: "2019-2-6"
categories: [
    "Blog",
]
menu: "main"
tags: ["Tyler Kluszczynski", "HAT", "Week 18"]
---

#### What's new this week?
This week, I finished the modules that were my responsibility. Hayden is finishing up the ECR/ECS modules before the presentation on Monday. This week, I also added documentation to my modules and completed the majority of the presentation for our group.

This week for the blog, I will cover the modules I created this sprint, along with where the documentation for all of the modules can be found.

#### Completed this Sprint
Week 1:
* admins_iam.tf
* alb_with_acm.tf
* gitlab_iam.tf
* launch_config.tf
* provider.tf
* state_sync.tf
* vpc.tf

Week 2:
* routetable.tf
* security_groups.tf
* Documentation for all modules (increased in line commenting and official project documentation).
* Sprint presentation.

#### Module Documentation
The documentation can be found in /DevOps/docsd/design/terraform_modules.md

The overall documentation covers our goals and a summary. Each module has its own section, including a list of variables, static content, concerns/problems, a description of the module, and outputs of the module.

Here is an example of the documentation for Route 53 with an ACM certificate:

```
***route53_with_cert***

Creates route 53 records along with an AWS ACM certificate and validates them.

Variables:
- zone_name - name of the route53 zone to create.
- acm_domain_name - domain_name used in certificate creation
- alternative_names - list of alternative names used in the cert creation

Static Content and Assumptions:
- DNS validation is used.
- TTL = 60.

Concerns/Problems:
- Certificate takes a long time to generate.
- NameCheap must be manually updated with new name servers still.
- Should move zone creation to a different file in future.

Description:
- Creates a route 53 zone, ACM certificate using DNS validation, then automatically populates the zone with the validation options required for validation.

Outputs:
- cert_arn = certificate ARN once complete.
```

The documentation is structured similar to function/class documentation for software development. An important point is listing the assumptions and concerns, as it gives us an outlook for what we could improve in the future for each module.

Some static content is required for modules, as some Terraform configuration changes greatly based on how each service is being implemented. For example, when using ALBs the redirect and default_action blocks may not exist for different types of listeners as seen in this example from our old Terraform file:

```
# Listener for HTTPS
resource "aws_alb_listener" "alb_front_https" {
  ...
  default_action {
	   target_group_arn = "${aws_alb_target_group.ecs-target-group.arn}"
     type = "forward"
	}
}

# Listener for HTTP
resource "aws_alb_listener" "alb-front-redirect" {
	...
	default_action {
    type = "redirect"
    redirect {
      port = "443"
      protocol = "HTTPS"
      status_code = "HTTP_301"
    }
  }
}
```
It is more difficult to make these kind of Terraform resources into modules, so they must be generalized less. For example, creating an "alb-redirect" module that states the module is used for redirecting http to https on an alb. Also, the module could be broken up into different components to promote reusability in the future. To reflect this, we update the module's documentation (as seen here):

```
***alb_with_target_group***

Allows for creation of an ALB with a set of target groups. This ALB is always going to redirect http to https.

Variables:
- target_group_name - the name of the target group.
- target_group_vpc - the VPC of the target group.
- alb_name - the name of the alb
- security_groups - a list of security groups to attach to the alb.
- subnets - a list of subnets to attach to the alb.
- ssl_policy - the ID of the SSL policy (amazon provides a list of ssl policy versions).
- certificate_arn - ARN of a certificate generated from AWS Certificate Manager.

Static Content and Assumptions:
- Port 80 will be used as a target group, and will redirect to port 443.
- Port 443 will be used as a target group.

Concerns/Problems:
- This module isn't as reusable as other modules, simply because of the nature of ALBs. ALBs generally require different content based on which type of listeners are used, and what the ALB is being used for.
- Can possibly break this module up into smaller modules in future.

Description:
- This modules creates a single target group, ALB, and 2 listeners. These listeners will forward traffic from port 443 to 80.

Outputs:
- None
```

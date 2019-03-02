---
title: "Alex's Blog Week 21."
date: 2019-03-1T12:10:08-08:00
draft: false;
tags: ["Alex Enaceanu", "BEATS", "Week 21"]
layout: 'posts'
---

# CIT 481 - SAPS
#### Week 21
For week 21 we started working on a monitoring solution for the SAPS. There are various ways to approach this need. We have chosen to go with Datadog. We will need to gather a multitude of metrics such as CPU and memory usage. Datadog makes this process relatively easy through their integrations console. However, for this to work, Datadog agent must be installed on the target machines. You can use the following command to install the agent on most Linux distributions:

    DD_UPGRADE=true bash -c "$(curl -L https://raw/Datadog/Datadog-agent/master/cmd/agent/install_script.sh)"

This can be done with autmoation software such as Ansible, and all Ansible will essentially do is run this command for you. Once the Datadog agent is installed on the target machine(s). We can configure the dashboard, but we will still need to grant Datadog access to the AWS environment. To accomplish this I initially tried by creating a new IAM user and policy to attach to the user with the following code:


    resource "aws_iam_group" "3rd-Party" {
    name = "3rd-Party"
    }

    resource "aws_iam_user" "Datadog"{
      name = "Datadog"
    }

    resource "aws_iam_group_membership" "3rd-Party-Membership" {
      name = "Adding-members-to-3rd-Party"

      users = ["${aws_iam_user.Datadog.name}"]

      group = "${aws_iam_group.3rd-Party.name}"
    }

    data "aws_iam_policy" "3rd-party-policy" {
      arn = "arn:aws:iam::aws:policy/ReadOnlyAccess"
    }

    resource "aws_iam_group_policy_attachment" "attach-policy-ReadOnlyAccess" {
      group = "${aws_iam_group.3rd-Party.name}"
      policy_arn = "${data.aws_iam_policy.3rd-party-policy.arn}"
    }

The above code worked without issue, however, further investigation revealed that an AWS IAM role is required instead of an IAM user. Luckily, we can apply a policy to an IAM role so we can reuse that portion of the above code.  An IAM role can be coded in Terraform with the following:

        resource "aws_iam_role" "test_role" {
          name = "test_role"

          assume_role_policy = <<EOF
        {
          "Version": "2012-10-17",
          "Statement": [
        {
          "Action": "sts:AssumeRole",
          "Principal": {
          "Service": "ec2.amazonaws.com"
        },
        "Effect": "Allow",
        "Sid": ""
        }
      ]
    }
    EOF
    }

In this example, we designate the use of an IAM role policy with the assume_role_policy argument, which is slightly different than a standard IAM policy. This same method can be used to create a role for other services, such as packer. A least privileged model can be applied via the specifications in the policy. I just learned this may be vastly different for containers...

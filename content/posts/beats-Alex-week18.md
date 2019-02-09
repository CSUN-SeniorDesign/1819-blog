---
title: "Alex's Blog. Week 18."
date: 2019-02-01T13:26:36-08:00
draft: false
tags: ["Alex Enaceanu", "BEATS", "Week 18"]
layout: 'posts'
---

# CIT 481 - SAPS
#### Week 18
This week I kept my focus on documentation. I had no meetings with any other teams. I did have an informal meeting with the IS department Chair who is the overall leader or stakeholder of the SAPS project on the IS departments end. We talked about onboarding new members for continuity since many of us are graduating this semester.

I continued working on documentation for an S3 bucket which we will use for access logs. It is named the Access Log Bucket which is deployed via code inside a Terraform module. That code specifies the security policy for the bucket via JSON code. Below is a sample of the documentation for the access log bucket
  # Access Log Bucket
  #### Notes
  * The access log bucket is just a "folder" in Amazon S3.
  * The security policy is written in JSON.

An Amazon S3 bucket is a public cloud storage resource available in Amazon Web Services' (AWS) Simple Storage Service (S3). Amazon S3 buckets are similar to file folders, store objects which consist of data and their descriptive metadata.

  Access Log Bucket Code:

      data "aws_elb_service_account" "main" {}

      data "aws_caller_identity" "current" {}

      resource "aws_s3_bucket" "terraform_log_bucket" {
        bucket = "${var.bucket_name}"

        versioning {
          enabled = true
        }

        lifecycle {
          prevent_destroy = true
        }


        policy = <<POLICY
      {
        "Id": "Policy1546882088739",
        "Version": "2012-10-17",
        "Statement": [
          {
            "Sid": "Stmt1546882082718",
            "Action": [
              "s3:PutObject"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::${var.bucket_name}/ALB/AWSLogs/${data.aws_caller_identity.current.account_id}/*",
            "Principal": {
              "AWS": [
                "${data.aws_elb_service_account.main.arn}"
              ]
            }
          }
        ]
      }
      POLICY
      }
  ---End Documentation example---

  And end blog, until next week . . .

---
title: "Hat Aubrey Week15"
date: 2018-12-07T19:15:50-08:00
draft: false
---

# Lambda #

This week, I was tasked to handle the automation of updating the containers hosted by AWS ECS. My objective was to:

- Be able to identify, and update between staging builds and production builds
- Be able to use version for easy rollback
- Gracefully update the application by creating new containers before deleting the existing containers.

For now, we will use the text file upload to s3 as a control for the version to be used.

The lambda will be triggered by the S3 text file upload. The textfile will contain the commit sha and the location or file name, will indicate if it's for staging or production. Production upload will trigger the revision of task definition and changing of the task def that the service is using. Once this happens, AWS Cluster will handle gracefully updating the application. 



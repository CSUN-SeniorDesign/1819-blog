---
title: "Project 5 Post 1 - Daniel"
author: "Daniel Hirahoka"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 10"]
date: 2018-11-02T20:26:21-07:00
draft: false
---

For this project we had edited project 4 to reflect what we were going to do in Project 5. While we did look for the cheapest build for our blog in Project 4 we were still using expensive components that could easily be dropped for a cheaper alternative. This involved dropping the use of EC2 instances and everything involved such as the ALB. This significantly cut down the price of hosting our site from around 70 dollars per month to maybe 3 dollars using our new design. The new design includes a Domain that uses Route 53 to point to our S3 buckets that host the contents. Each bucket would host our content and other buckets would point to the root bucket if we wanted domain Aliases such as www. and blog. they would each be a bucket that points to our root bucket. Using Route 53 and S3 this is easily possible and the only thing we need to host our blog. Implementing an extra feature such as Cloudfront was also neccessary to be able to hand and push our content to people who were accessing our site. 

The only issue was we had to implement these features using Terraform and use CircleCI to push our content and the Terraform files. This took the most time, but it was pretty straight forward. We modified old Terraform files to reflect the changes, dropping what wasn't needed. As for Cloudfront we used AWS to create the Cloudfront distribution but realized that we needed this portion to also be done in Terraform. So we had to delete the old Cloudfront and remake it using Terraform, a skeleton file was foud on the Terraform documentation and then it was a matter of filling it what we needed. We are still currently working on the site trying to finish the Cloudfront portion which takes the longest because it takes approximately 15 minutes for it to shutdown and startup.
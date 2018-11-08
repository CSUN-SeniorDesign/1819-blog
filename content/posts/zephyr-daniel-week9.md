---
title: "Project 4 Post 1 - Daniel"
author: "Daniel Hirahoka"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 9"]
date: 2018-10-26T13:59:29-07:00
draft: false
---

For Project 4 the steps I took to calculate were pretty straight forward. I used Project 2 as a framework on what to expect should be included in the pricing. Since the restrictions seem to align with what we already do on our projects it seemed like the correct path to take. I broke down the project into sections that were needed, this consisted of having a Route 53, S3 bucket, EC2, VPC and possibly an ALB.

I began calculating how much it would cost to have Route 53 for traffic to our site. One of the requirements was to see the price for 1000 requests per day, while I have this in mind while I'm doing the pricing it's not really asked in the AWS calculator or not that I can see. When doing the calculations I was asked about hosted zones, traffic flow and about queries. I filled in the information about Hosted Zones as we only needed one and the Traffic Flow. Reading about Traffic Flow it's basically the policies we are going to use and we only needed one to have the site running. In total it came out to be around 50 dollars.

Next requirement stated that the blog content needed to be hosted on AWS which is something we already do. S3 bucket serves that purpose and doing the calculations for that was straight forward. I had to come up with how many requests we needed and the amount of storage. Overall the price is very insignificant compared to other services. We can look for alternatives here as there are other option that AWS offers that will help host our blog content but sticking with S3 is probably better for our objective and cheaper.

I continued to do this for every section until I had a total. I'm currently looking into alternatives to certain services and dropping certain services. For example if we are only trying to setup a static blog you can definitely tackle this issue without an EC2 cutting some costs. I also began working on re-doing project 0 - 2. I felt that in certain areas I lacked a certain understanding and doing the projects over again will certainly help me out.


---
title: "Erik's Blog. Week 13"
date: 2018-11-23T18:05:53-08:00
tags: ["Erik Blomquist", "BEATS", "Week 13"]
draft: true
---

# Project & EC2 On-Demand vs Fargate Potential costs
We started the week by switching from GitHub to GitLab so that we can keep our repositories private. I was assigned the task on GitLab to compare the potential costs for instances to host our docker containers. I compared EC2 On-Demand (t2.medium/t2.large) and Fargate prices.

## EC2 On-Demand
You only pay for what you use with EC2 On-Demand instances. There are cheaper alternatives such as spot instances, dedicated hosts and reserved instances but they are not as reliable and we are not ready to signup for any contracts yet. I compared both t2.medium and t2.large costs with the following formula:

Monthly cost = Price per Hour x Hours of month

### t2.medium
- vCPU: 2
- Memory: 4 GiB
- Price per Hour: $0.0464

monthly cost = $0.0464 x 720 = **$33.41**

### t2.large
- vCPU: 2
- Memory: 8 GiB
- Price per Hour: $0.0928

Monthly cost = = $0.0928 x 720 = **$66.82**

(Source: https://aws.amazon.com/ec2/pricing/on-demand/)

# Fargate
Same as EC2 On-Demand instances Fargate also uses pay for what you use. The AWS website provides simple examples to calculate the potential costs.
We decided that our tasks will use 4 ECS tasks running at all times for a month where each task uses 0.25 vCPU and 1 GB Memory.

**Monthly CPU charges:**<br/>
Total vCPU charges = # of Tasks x # vCPUs x price per CPU-second x CPU duration per day (seconds) x # of days

Total vCPU charges = 4 x 0.25 x $0.00001406 x 86,400 x 30 = **$36.44** / month

**Monthly memory charges:**<br/>
Total memory charges = # of Tasks x memory in GB x price per GB x memory duration per day (seconds) x # of days

Total memory charges = 10 x 1 x 0.00000353 x 86,400 x 30 = **$91.50** / month

**Monthly Fargate compute charges:**<br/>
Monthly Fargate compute charges = monthly CPU charges + monthly memory charges

Monthly Fargate compute charges = $36.44 + $91.50 = **$127.94** / month

(Source: https://aws.amazon.com/fargate/pricing/)

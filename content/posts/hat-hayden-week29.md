---
title: "Hat-Hayden-Week-29"
author: Hayden
date: 2019-05-03T19:14:29-07:00
tags: ["Hayden Wilcox", "HAT", "Week 29"]
draft: false
---

Overview:
- Research into setting up a Kubernetes cluster
- Research and implementation of a Rancher Kubernetes Cluster

What I did:
- Began work on a Kubernetes Cluster
- After struggling to decide on a design philosophy and realizing a fully automated, modularized terraform setup was beyond my time and skill level, I decided to use Rancher orchestration in order to get an infrastructure set up and understand the technology I'm working with
- Following the documentation, I downloaded the quickstart repository from Rancher's official GitHub
- From there I edited a terraform.tfvars file to include my AWS keys, setup some credentials and specify the number and type of nodes running in my cluster (2 worker nodes and 1 node with type "all" (worker, etcd, controlplane))
- I needed to setup a default vpc and subnets for us-west-2, the aws region we've been working on this semester
- Once terraform apply was finished it printed out an IP address to access my personal Rancher dashboard
- I set up a workload using the image given by the Rancher quickstart guide
- I set up an ingress rule for port 80 and the dashboard generated a new IP address with an xip.io domain (a specialized domain that extracts and returns the IP address from a DNS zone, allowing you to access virtual hosts on a development web server)
- Once the domain was populated I was able to access the container being run on my Kubernetes cluster. It said what the IP address was and how many nodes were currently running.

Issues:
- Originally I was going to work on a Kubernetes cluster with my team. However, it was decided that we would all create different infrastructures which led me to be confused as to how to proceed.
- I realized that I hadn't set up a default vpc or any subnets for my region. I chose to go with the region I was working in last semester us-west-1 and realized that there were a lot of items that hadn't been cleaned up. After deleting a lot of them however, I realized that I had deleted the default subnets on accident as well. I also realized that EKS was not available for the us-west-1 region either. With all of this in mind, I decided to go back to us-west-2, and create the default vpc and subnets there.

Next Time:
- I am going to creating a Docker container with a simple html page and vhost setup, and load it up with Rancher.
- Once the above is finished, I will be creating an EKS cluster using the console and my personal image.
- If there is still time after this, I will attempt to create some Terraform files to be able to automate the process of setting up the EKS cluster. If all goes well, I may even modulate this process.
- Even if I am unable to complete this before the next presentation, I will strive to complete it anyway, as it will be good practice and something to show potential employers in the future.
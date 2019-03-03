---
title: "Matabit Shahed Week 21"
date: 2019-03-02T13:27:23-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 21"]
layout: "posts"
---

# Monitoring

Due to the peculiar nature of how Fargate works, monitoring is rather complicated. It isn't as simple as installing an agent on a host, because Fargate doesn't have dedicated hosts. We have the option of just monitoring with AWS CloudWatch, but that doesn't give us enough practice with third party tools.

However, Datadog has officially partnered with AWS to create a unique monitoring solution for Fargate. Since the Student Developer Pack from Github enables us to use Datadog for free for up to two years, we will be chosing this monitoring solution for the interim. 

Prometheus, as of now, is not being considered for our infrastructure monitoring solution.

# Automating provisioning

Terraform has helped us immensely with automating the provisioning process of our infrastructure. However, currently we are experiencing the issue that we need to go and provision every single module individually, which if not documented correctly, can result in issues during the provisioning process. Hence our goal is to be able to tear down and build up the entire infrastructure with a single command, while still keeping the option of manually reinstantiating a single resource.

Unfortunately, I haven't made much progress in this regard, hence why I have no exmaples yet.




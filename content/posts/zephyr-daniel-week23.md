---
title: "Zephyr Daniel Week23"
date: 2019-03-15T10:40:31-07:00
draft: false
tags: ["Daniel", "Zephyr", "Week 23"]
---

# Week 23

This week I was tasked with working with Prometheus and getting that running on our EC2 instances. Since I've never used Prometheus before I started the first week trying to just learn and implement the starting file that Prometheus has on there documentation. This week on a VM I decided to try to do it like we did our exercises with Ansible, I started up a docker container where I can test having the Prometheus setup on. I think this is a good idea as if I have any troubles I can have an easier time pin pointing the errors in a clean environment than my regular virtual machine that has a bunch of stuff that can interfere with it. After installing some dependencies I then installed Prometheus.
```
tar xvfz prometheus-*.tar.gz
cd prometheus-*
```
Following there documentation I installed the latest version of Prometheus. I then proceeded to configure the Prometheus.yml file to just monitor itself. During the weekend I'll be try to change this file to meet our needs in regards to monitoring. The example yml file looks as follows:
```
global:
  scrape_interval:     15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
```
The file to my understanding is split up into 3 sections. The global section which applies to everything and in this case will tell Prometheus how often it will look and update itself with the scrape interval. The evaluation interval tells it how often to look into the rules. The rules file will tell it the rules it must follow.


# Tasks as of Now
As of now I have been trying to read up on how to further modify the Prometheus.yml file and see what else I can do to get more metrics. Since I'll be using this for AWS EC2 if there is anything specifc that I need to be aware of. Afterwards I'll be doing some more CircleCI and implementing my group members stack in a Docker file into the CircleCI.

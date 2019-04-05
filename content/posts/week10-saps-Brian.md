---
title: "Brian's Blog. Week 10 Spring"
date: 2019-4-1T15:57:18-07:00
draft: false
layout: 'posts'
tags: ["Brian Sandoval", "saps", "week10"]
---
# Brian's Blog Week 10 Spring
## Overview
This week I was able to work along side Erik in setting up part of the ansible playbook that we will be using in order to launch prometheus and grafana. Currently I am working on another playbook that will allow me to setup the grafana monitor boards so that we can have visual representation of prometheus. So far what I have is pretty much setting up the datasource for prometheus as an example of the following:
```
grafana_datasources:
  - name: prometheus
    type: prometheus
    access: proxy
    url: 'http://{{ prometheus_web_listen_address }}'
    basicAuth: false
```
What the datasource should do is set up prometheus and the url that it is going through the aboce as well checks to see if there is some form of authentication. This initializes the data information flowing through we as well need to spit out the information on to a dashboard so the following snippet of code for the playbook shou;d take care of setting up the dashboard:
```
grafana_dashboards:
  - dashboard_id: 111
    revision_id: 1
    datasource: prometheus
```
So far all of our cpu metrics along with the flow of traffic on our network are the biggest worries for monitoring at the moment we as well want to be able to keep metrics about our memory and storage. I still need to reach out to some of the other group members about other ideas. One of the things that we can continue to work on for the development of the monitoring system is to implment some sort of alarm that will notify us in slack so that we are able to get alerted if there is some critical system that goes down or if there are any warnings we need to keep tabs on. I will try to setup some alarm of some kind by the end of this weekend if not the start of next weekend depending on how much time is left over.

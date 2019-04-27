---
title: "Hat Aubrey Week28"
date: 2019-04-26T21:03:43-07:00
draft: false
---

Kubernetes Basics

A kubernetes cluster is an orchestration system for the lifecycle of application containers. A kubernetes cluster consists of the master and the node. The master will have the k8 api that will serve as the main point of contact for the end user and the node. Each node will have an agent called kubelet and the containers. The master is responsible of maintaining the "desired state" of the application. Similar to AWS ECS, if we defined that the application needs 3 replicas, the master will make sure that there are 3 replicas. The master will communicate with the kubelets to get the current status and also to bring up more containers if necessary. If a node goes down, the master will be responsible to push the load to another node. 

The desired state of the application is defined in a .yml file. This is similar to a task definition in AWS ECS

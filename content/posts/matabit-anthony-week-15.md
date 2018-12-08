---
title: "Anthony Week 15"
date: 2018-12-07T22:06:43-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 15"]
layout: 'posts'
---
# Week 15
This week went by fast. On Monday, we had a stand up and some of the developers had issues with a few things. One with git; mainly just getting the workflow down. Another issue was with Guzzle to call the Waldo API. I asked Luis, our backend mentor about this issue. It seems related to Windows and how it attempts read a Cert file that doesn't exist. To remedy this we use the code below to call the Waldo API. 
```php
$client->request('GET', '/', ['verify' => false]);
```

## Server stuff
I've put off the ECR/ECS deployment for now because I was busy handling other tasks at META+LAB and other classes. The team had a final presentation the same time as mine so I opted to provision a [playbook](https://github.com/digitalsoba/classroom-profiles-ops/tree/master/ansible/playbooks/laravel) to deploy their project on a lightsail instance. This is a temporary solution for now until I get the ECR down. This playbook will configure the instance for a LAP stack (without mysql because RDS is handling that). It'll also install the dependencies for Laravel and configure the proper Apache modules. It also clones to project and sets a `dev` environment/block. After provisioning the server I used Let's Encrypt to generate an SSL cert. 

I've also created a lightsail module just in case I need to provision multiple instances. 
## Drone's cloud
[Drone](https://cloud.drone.io) just released their CICD tool as a service for open source projects so I destroyed my instance to save some money. 

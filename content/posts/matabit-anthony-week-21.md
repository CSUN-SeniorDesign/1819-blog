---
title: "Matabit Anthony Week 21"
date: 2019-03-01T22:56:39-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 21"]
layout: 'posts'
---
# Short week 21
This week I didn't met with the rest of my team, caught a cold and didn't get much done. I'd throw in the Prometheus and Grafana stack this week even though I finished it Sunday. This was mainly used to test out the stack as I've never had experience working with it. 

## Setting up 
I've cloned this [repo](https://github.com/stefanprodan/dockprom.git) from Github to quickly bootstrap the project. I've when with the docker route just so I can't get a quick demo of it working on my mac. It's powered with docker-compose so I only had to use one line to bring the stack up. Within seconds I had the whole monitoring stack up. This stack includes Grafana (Visualizations), Prometheus (Metrics scrapper), Caddy (Proxy), Node exporter (metrics), cAdvisor (also metrics), and Alertmanager. 

## Deploying with Ansible
Now that I had a quick demo on my local machine I thought it was time to put it in "Production". I've written an ansible playbook to deploy the application with the following roles: Install docker+docker-compose, clone the repo, install Nginx, proxy Grafana port 3000 with Nginx, and install let's encrypt cert for Nginx. 
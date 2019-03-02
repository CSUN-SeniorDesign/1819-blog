---
title: "Hat-Hayden-Week-21"
author: Hayden
date: 2019-03-01T19:23:48-08:00
tags: ["Hayden Wilcox", "HAT", "Week 21"]
draft: false
---

At the beginning of the week I was assigned the task of setting up a monitoring system for our system. The main plan was to set up Datadog due to it's low memory cost. However, after seeing Anthony's presentation and how he managed to get Prometheus and Grafana working on a t2.micro instance, I consulted with him after class as to how he accomplished this. I found out that he used a very specific Docker image that he found on the GitHub that reduced the memory usage such that it would allow for this. After thanking him, I presented it to the team as a possible option. We agreed that it seemed like a good idea worth trying, but I decided to start with Datadog as it seemed simpler to set up.

I haven't had much experience with Docker in any form, and the majority of my experience with it was with Docker-Compose. However, after the last sprint my teammates never want to use compose again. The reasoning for this, is the amount of issues it caused during networking. Trying to get the containers to communicate with each other was very difficult and was the source of many of our problems. With this in mind, I had to do some research into Docker itself in order to be able to use it properly. At this point, we decided that the best method to use would be a multi-stage build Dockerfile, and adding the Datadog image to the Wordpress image utilizing an extra FROM statement. This would in effect, allow us to use multiple images like with Docker-Compose, but hopefully without the networking issue.

Due to unforeseen circumstances during the week, I was unable to get far with this. I was able to research Docker and how to add Datadog to the image file, but have not gotten a chance to truly test it yet. This will be done during the weekend.
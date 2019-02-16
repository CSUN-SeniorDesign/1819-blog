---
title: "Hat-Hayden-Week-19"
author: Hayden
date: 2019-02-15T16:48:13-08:00
tags: ["Hayden Wilcox", "HAT", "Week 19"]
draft: false
---

This week was unfortunately very slow for me. After the presentation and wrapping up of the previous sprint, I asked my teammates what I should work on next. After a discussion, it was determined that there was not enough work to go around for the first part of this sprint, and there was nothing that they needed me to directly work on. I was advised however, to begin researching and practice using ELK stack for our monitoring purposes. We decided on this one as it is slightly more common to see running on bigger systems and employers would like to see it on a resume. I began to look into look into it, and was getting ready to set up a docker to install it, when I noticed a crucial flaw. A standard ELK stack requires approximately 8GB of RAM to run, with Elasticsearch (the E of ELK stack) taking up 4GB on its own. Given that running a t2.medium instance type with 4GB of RAM is $38 a month on its own, we simply could not afford to run the ELK stack on the server. ELK stack is also known for being used for monitoring a single large server as opposed to several smaller interconnected ones such as ours so that was another factor in our decision not to use it.

After this was decided, I was told to look into Prometheus for our monitoring system using Grafana as a graphical display. This would have been slightly more manageable, at around 2-3GB, but still a little much for our system to handle. It was decided that we needed to shave some things off, as the project we are working on is a fairly simple Wordpress server, and we already have too much infrastructure as it is. Modularizing all of the previous project code was mostly for practice in understanding Terraform modules, and we don't actually need to use them all, such as the ASG. 

At this point however, I was pulled off of researching Prometheus and told to start helping with the ECS, which we still are having trouble getting up and running fully. At the time of writing this, I unfortunately haven't been able to help much with it due to time constraints and lack of skills, but over this weekend I hope to be able to catch up and help out my team some more.
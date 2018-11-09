---
title: "HAT-Hayden-Blog-11"
author: "Hayden"
date: 2018-11-09T14:15:19-08:00
tags: ["Hayden Wilcox", "Zephyr", "Week 11"]
draft: false
---

For week 11, our group was mostly focused on preparations for our work on the project for the rest of the semester and into next. Our first order of business was to meet up with eachother and exchange information, as we had not had much in the way of contact with one another over the past few weeks. Then after meeting with the computer science team that we will be working with, we were informed that professor Nerces Kazandjian had taken an interest in our project. We decided to go together that day to meet with him in the MetaLab and take notes on his specifications. He did not have many, save to be as conservative as possible with the infrastructure. Together we made a preliminary plan of what infrastructure we were going to create and created 2, 2-week plans based on what we wanted to create.

At this point, we needed a way to communicate and share the work we were going to be creating. We decided on using GitLab for private repository posting, and Slack for direct communication. Tyler handled all of the setup, down to connecting the two services so that updates to the GitLab appear as alerts in our Slack server, which has already proved very useful. My responsibilities here were mostly to sign up for and familiarize myself with GitLab and Slack, and to leave my previous AWS organization in favor of the current one. We also needed to encourage the computer science team to create accounts with these services so we can communicate and operate with them.

As the week drew to a close and all channels were being finalized, I created a baseline ECR for our group on GitLab. We were initially going to use CircleCI for our continuous integration purposes, then decided to switch to Jenkins, another very popular CI service when we realize that GitLab did not support CircleCI. I gathered some documents on Jenkins and began to look into it, but we later decided to use GitLab's built in CI service, which is limited, but cheaper to utilize.
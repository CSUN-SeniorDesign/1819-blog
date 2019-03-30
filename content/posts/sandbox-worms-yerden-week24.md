---
title: "Sandbox Worms Yerden Week24"
date: 2019-03-29T22:45:32-07:00
draft: false
Categories: [AWS]
Tags: ["Yerden Zhursinbek", "Sandbox Worms", "Week 24"]
---

It was a little rough, getting back to work and focusing on stuff after being absent for couple of weeks, however practising in class assignments helped me to deal with this week's assignment without much pain. As one of the project tasks is to install Prometheus on the SSH bastion host to monitor the EC2 instances, it was a good practice. I liked working with it since it has only one .yml configuration file and is pretty easy to configure. 
However on the next stage when I was trying to cennect to localhost it kept sending th error message "Failed to connect to localhost port 9100: Connection refused", despite the port was exposed correctly. After playing with it and running couple more docker containers It finally worked and I was able to curl the localhost. 
Since we will be installing it on bastion host I made a little reasearch on it. Eventually, ssh bastion host will enable secure connection to our EC2 instances without exposing environment to the internet and ease access to them via ssh. 
For the next sprint, i will be looking forward setting up bastion host and install prometheus on it. There is still a lot to catch up.

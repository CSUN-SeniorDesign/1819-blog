---
title: "Zephyr Daniel Week25"
date: 2019-04-05T18:20:22-07:00
draft: true
tags: ["Daniel" , "Zephyr" , "Week 24"]
---

# Week 25
This week I continued to work on the ECS cluster for the project. I setup the task definition to determine what we want inside a container. There is a docker image that we pushed to the ECR and we are using that for our image in the docker image line in our task definition. Following a guide, the next step would be to setup a service definition which I think is mandatory for this to work properly. My understanding is I will apply some sort of service to my container it will then trigger events that will run the ECS cluster. After these triggers I should see on the Amazon dashboard my cluster starting up. I have to do this part and see if I can finish it by this weekend.

# Tasks as of Now
I have to finish the ECS cluster terraform files as I am a little behind on finishing them. I got busy with other class projects and mid terms but over the weekend I should be able to finish this before having to present. I'm going to be applying these new terraform files to our github where it's linked to the circle CI and see how the two interact. Right now the CircleCI sets up our ec2 instances but adding this might complicate things, I'm not sure. Adding this and working out the bugs will be my goal for the rest of the weekend and seeing if circleCI can run correctly. Another thing I want to expand on is our Prometheus, it was something that was really interesting when we worked on it as an activity for class and implementing it for our project. RIght now it seems a little bare bones only giving us some metrics about our containers but i want to expand it so it can give us more data, just have to figure out how.s 

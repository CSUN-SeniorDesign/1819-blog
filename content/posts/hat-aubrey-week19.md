---
title: "Hat Aubrey Week19"
date: 2019-02-15T18:35:23-08:00
draft: false
---

Things I learned last night: We can still use terraform to create the initial multi-container services, use gitlab ci/cd to constantly push new image of the container with the updated source code, run ad-hoc task to do database migration when needed (say when a new plugin is installed on wordpress in the dev environment).

1. the docker-compose.yml == service
2. each section in the docker-compose.yml == task definition  
3. aws provides different networking option. First option is similar to the default in docker where we each port in the container is bound to a port in the host. so, if a container is listening on port 80, on the host, you can bind it to port 8000. When you go to the localhost:8000, it maps to the port 80 of the container.  
4. another option we can look into for networking is using awsvpc. I didnt get a chance to try it out but i started deploying in the code. with awsvpc as the networking mode, each container will be part of the VPC that we have. It means that we can assign it to the private subnet we created. With this, we dont have to worry about mapping ports with the host.  
5. either way should work, i am trying out both ways to understand further.
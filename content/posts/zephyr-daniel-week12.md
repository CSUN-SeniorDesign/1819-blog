---
title: "Irrigation Post 2 - Week 12"
author: "Daniel Hirahoka"
date: 2018-11-16T13:16:54-08:00
draft: false
tags: ["Daniel, "Zephyr", "Week 12"]
---
# Week 12

For week 12 we have been working on getting together a testing environment for the two groups we are working with. As well as setup a foundation for them to collaborate with us when they are ready. The foundation setup so far is a discord server where we have our own channels along side the other groups channels. This way we are able to directly communicate with any member of the team in a quickly manner. The other consists of creating a repository for the Irrigation group separate from the Encryption group or vice versa. We are still discussing this as a group but we have thought that this might be a good idea to setup early before we begin actually setting up the live testing environments.

## Task as of Now
We have discussed how we should begin setting up a foundation for both projects and have decided that having testing environments set up is a good solution. Both the Irrigation team and RCrypt team have confirmed that this is something they would like so we are proceeding with it.

To setup the environment we are going to use Docker to create containers that we can push to EC2 instances. This way they can have set of requirements in the Dockerfile and can push that same container whenever they want to start fresh.

Both the Irrigation and RCrypt Dockerfile wont have much substance in them as we are still discussing what is needed in each container. As of now the only requirement for the Irrigation Dockerfile is having a LAMP stack so it's available to host a website. This is done so far by referring an image that has LAMP already built in which I found in the Docker hub.

The Dockerfile looks like this right now for the Irrigation group:
```
FROM mattrayner/lamp:latest-1604

CMD ["/run.sh"]
```
This container contains an image that has LAMP ready to use, while the last CMD line is something I have to look into it needs to be part of the container.

As for the RCrpyt, this other Dockerfile contains a bit more. We did have more defined requirements for this container, we needed at least to have GIT installed as well as a Java JDK. After some research I found an image that has a Java JDK on an alpine image and all I had to do was install GIT.

The Dockerfile :
```
FROM anapsix/alpine-java

RUN apk add "git"

RUN git --version && java -version
```

Running the final command of the container confirms that the JDK is installed as well as GIT.

### What now?

As of now this is a small foundation we have created, we are going to move forward and setup up terraform requirements and CircleCI. As we get more and more requirements for the containers I will add onto them and post different versions of it.

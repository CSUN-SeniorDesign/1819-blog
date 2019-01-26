---
title: "Shahid's Blog. Week 14."
date: 2018-11-29T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 14"]
layout: 'posts'
---

## Shahid's Blog. Week 14
#### What's new this week?
I had to set up docker on my machine in order to test the bash scripts that I created from a fresh OS install. Before I was creating a new VM each time I changed my script. Rather than doing it that way, it made more sense to use a docker container. Not only was it faster to start from a fresh OS with docker but it was done in one step.

## Docker for Automation Testing

### Installing Docker
1. Update the package list and install required software.

  ```
  sudo apt update && \
  sudo apt install linux-image-extra-$(uname -r) linux-image-extra-virtual apt-transport-https ca-certificates curl software-properties-common
  ```

2. Add GPG key to the Docker repository then add it to APT sources.

  ```
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
  ```

3. Update the package list again and make sure to download from docker repository rather than ubuntu. Then install docker.

  ```
  sudo apt update -y && \
  apt-cache policy docker-ce && \
  sudo apt install -y docker-ce
  ```

4. Add the user to the docker group to avoid having to use sudo on every command.

  ```
  sudo usermod -aG docker ${USER}
  ```
  or
  ```
  sudo -i
  sudo usermod -aG docker master
  ```

### Setting up Docker for the first use
1. Run the command to check which version of ubuntu to download.

  ```
  docker search ubuntu
  ```

2. Once you know which version you want, install it using the pull command.

  ```
  docker pull nameOfImage
  ```

### Creating a Docker image using a Dockerfile
1. First start by creating a new directory for the Dockerfile.

2. Use an editor to create the Dockerfile

  ```
  nano Dockerfile
  ```

3. Start the Dockerfile with the ```FROM``` command that uses an operating system as a base.

  ```
  FROM ubuntu:18.04
  ```

4. Use the ```RUN``` command to run the linux command to download and install software

  ```
  RUN apt-get -y install apache2
  RUN ./home/install.sh
  ```

5. Use the ```COPY``` command to move files form the host machine

  ```
  COPY install.sh /home/install.sh
  ```

6. Once you've finished adding commands to the Dockerfile, build the Docker image.

  ```
  sudo docker build -f image-name -t tagid [path/from/root/to/image]

  sudo docker build /path/to/Dockerfile -t image-name
  sudo docker build - < /path/to/Dockerfile -t image-name
  ```

  example:

  ```
  sudo docker build -t software-irrigation -t 1.0 ~/Desktop/docker-test

  sudo docker build --rm=true -t software-irrigation:1.0 .
  ```

7. Remove a docker image. As long as a couple unique characters from the image id is used, the command will find the entire id and remove it for you.

    ```
    sudo docker rmi [imageid]
    ```

### Creating/Running/Managing a Docker containers/images.
1. Check which images you have.

  ```
  sudo docker images
  ```

2. Run the image as a container interactively (Using a shell).

  ```
  sudo docker run -it software-irrigation
  ```

3. Run the image as a container non-interactively.

  ```
  sudo docker run -t -d software-irrigation
  ```

4. Start and stop a container.

  ```
  sudo docker start containerID
  sudo docker stop containerID

  # Stopping all docker containers
  sudo docker stop $(sudo docker ps -aq)
  ```

5. Manage containers.

  ```
  # Check all active and inactive containers.
  sudo docker ps -a

  # Remove a stopped container.
  sudo docker rm containerID

  # Removing all docker containers.
  sudo docker rm -f $(sudo docker ps -aq)

  # SSH into a running container
  sudo docker exec -it container-name /bin/bash
  ```

6. Manage images

  ```
  sudo docker rmi imageID

  # Remove all images
  sudo docker rmi -f $(sudo docker images -q)
  ```

---
title: "Irrigation Post - Week 26"
date: 2019-04-12T14:48:23-07:00
draft: false
tags: ["Daniel" , "Zephyr" , "Week 26"]
---

# Week 26
This week I am tasked with working on the Terraform and learning how my group member Tonny had setup our infrastructure using Terraform. Using his documentation try recreating it and seeing if I can implement it myself. I've started with reading his documentation and then creating each module one by one. Starting with the credentials and onto the VPC. I'm familiar with terraform now because I was tasked to do the ECS cluster using Terraform last sprint but now I'll be able to see how everything is setup now.

Otherwise I have worked on the Dockerfile assignment. I really like Docker and what it does, and this assignment was very enjoyable for me.

The basic requirements for the assignment were to install an non php apache base image, meaning we had to use a ubuntu or centos image. Installing php and apache using the Dockerfile and any dependencies it needed. Finally open port 80 and take a screenshot of the local host site.

I decided to use an ubuntu image for this project as I am most familiar with ubuntu and the commands needed to install a package.
```
FROM ubuntu:18.04
```
This line in our Dockerfile will get us the ubuntu 18.04 image for us to mess around in.
Afterwards we want to update our files and then install apache and php.
```
RUN apt-get update && apt-get install -y apache2
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y php
```
The first line will update our files then proceed to install apache and having it auto accept the download. The next line has the DEBIAN_FRONTEND=noninteractive part where it will make it so we don't need to interact with the prompts that php would usually ask us for. It will just give it default options instead.

Now another requirement was to make our own index file that has our name and major in it. I had a working directory for the Dockerfile where I had two files. One file named index.html and another info.php. The index.html file contained the html code for the basic title and paragraph for what was asked for. We also want to put this file into the directory inside our docker container where it will be accessed. The same with info.php to show that we have php enabled.
```
COPY index.html /var/www/html/
COPY info.php /var/www/html/
```
The COPY command will copy the index.html file and the info.php file from the current directory into the html directory inside the container.

An error that I ran into while trying to get this running was that apache was having a hard time starting up. The error involved saying it couldn't resolve the servername. To fix this I had to append the apache2.conf file with "servername localhost" this then solved the error and started apache no problem.
```
WORKDIR /etc/apache2/
RUN echo "ServerName localhost" >> apache2.conf
```
WORKDIR put me where the file was and then the next line appended the file with what I needed to add.

Finally we started apache2 and exposed port 80
```
CMD /usr/sbin/apache2ctl -D FOREGROUND

EXPOSE 80
```

# Tasks as of now
I need to continue working on the Terraform stuff and follow the documentation while learning why everything does what it does. While also recreating the infrastructure myself.

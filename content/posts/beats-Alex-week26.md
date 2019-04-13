---
title: "Alex's Blog Week 26."
date: 2019-04-12T21:00:00-07:00
draft: false
layout: 'posts'
tags: ["Alex Enaceanu", "BEATS", "Week 26"]
---
# CIT 481 - SAPS
#### Week 26
We have used Docker in class for assignments and our own projects. This week we will go over how to automate an installation of a LAMP stack on a Linux image using Docker. You will of course be requiring root access to the Linux machine and a basic understanding of the Docker software.

### Docker Installation
First, you'll need to install Docker. I'm using Ubuntu, therefore the command will be:

      # apt-get install docker docker-compose

Once installation is complete, the Docker service will be started automatically. That means we can get started! Do so by making a 2 directories  for the project:

      $ mkdir -p dockerized-lamp/DocRoot

We will also make a page that outputs our php info as is standard with a prelimenary LAMP install:

      $ echo "<?php phpinfo(); ?>" > dockerized-lamp/DocumentRoot/index.php

Now we can get started on our **docker-compose.yml**! These are pretty plug an play as far as writing the code goes. There is great documentation all over the web including Docker's site on how to write your docker-compose.yml, so I will just cover some basics. For example the definitions' will be specified in the **services** section. In our code below, the image is specified where it says image, (told ya it was straight forward) the links section contains

      version: '3'
      services:
        php-apache:
            image: Ubuntu 18.04.2
            ports:
                - 80:80
            volumes:
                - ./DocumentRoot:/var/www/html:z
            links:
                - 'mariadb'
        mariadb:
            image: mariadb:10.1
            volumes:
                - mariadb:/var/lib/mysql
            environment:
                TZ: "Europe/Rome"
                MYSQL_ALLOW_EMPTY_PASSWORD: "no"
                MYSQL_ROOT_PASSWORD: "rootpwd"
                MYSQL_USER: 'testuser'
                MYSQL_PASSWORD: 'testpassword'

                MYSQL_DATABASE: 'testdb'
      volumes:
        mariadb:

  Now you can test out your LAMP stack Docker install by viewing the phpinfo.php page in a browser.

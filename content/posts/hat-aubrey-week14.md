---
title: "Hat Aubrey Week14"
date: 2018-11-30T20:34:51-08:00
draft: false
---

# Docker Compose #
For this week, I started working creating a proof of concept for using docker compose as a development environment for our developers.

Docker compose is an easy way to bring up multiple containers in a consistent environment. It also allows to define networking so that the containers can communicate with each others. Docker compose uses a file called docker-compose.yml as its configuration file. To run docker compose:

			docker-compose up
To bring down the containers, press CTRL+C. To cleanup issue:

			docker-compose down

The docker-compose.yml will consist of the images of the container to be brought up. It may reference an exisiting dockerfile or if the image is simple enough, the entire content may be written in the docker-compose.yml file.

		version: '3' #indicates the version to use
		services: #the sublevel after services indicate an image
		  php: #the image for a container that will be called php
		    build: 
		      context: ./php #specifies where the dockerfile is at
		    ports:
		      - "80:80"
		      - "443:443" #exposes the port
		    volumes:
		      - ./php/src:/var/www/html #mounts a folder in the host computer to the container to allow immediate testing of codes to be developed without needing to bring up a new environment
		
		  db: #image for the sql server. in this case, because everything will be default, no additional dockerfile  was used.
		    image: mysql
		    command: --default-authentication-plugin=mysql_native_password
		    restart: always
		    environment:
		      MYSQL_ROOT_PASSWORD: example
		
		  adminer:
		    image: adminer
		    restart: always
		    ports:
		      - 8080:8080
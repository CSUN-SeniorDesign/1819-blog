---
title: "Hat Aubrey Week17"
date: 2019-02-01T21:22:55-08:00
draft: false
---

# Docker Compose and WordPress #

My group started to implement modules on our terraform source code. I kept in touch with the CS group to help them out in setting up their development environment.

As a the new direction from the client requires the use of wordpress, I helped one Computer Science group member install MySQL, Apache and PHP. He was able to install all 3 of them but he could not get it to work. There were multiple problems:

1. The wordpress files were not copied to the document root of Apache. I readjusted the Apache config to point to where the wordpress files are.
2. The wordpress configuration did not have the correct database name, server name, username and password. I was able to guess the database name and password.

After the initial setup, the entire CS team were able to install the LAMP stack on their laptops. This gives them some time to be familiar on developing web site using wordpress and PHP. 

I also explained and demonstrated the concept of container using docker compose. I showed them how easy it was for me to bring up a development environment without going thru the entire setup that they did in their VMs. I showed them how I can ship the entire container instead of just the code. I explained that my setup is as close to production as possible because I have the Front-End in a different container than the SQL database.

### Docker Compose ###

- Persistent database volume for the MySQL container
- Front-End will use wordpress


		 version: '3.3'
		
		services:
		   db:
		     image: mysql:5.7
		     volumes:
		       - db_data:/var/lib/mysql #mount the volume db_data. allows me to delete the container without losing the data in MySQL
		     restart: always
		     environment:
		       MYSQL_ROOT_PASSWORD: 
		       MYSQL_DATABASE: 
		       MYSQL_USER: 
		       MYSQL_PASSWORD: 
		
		   wordpress:
		     depends_on:
		       - db
		     image: wordpress:latest
		     ports:
		       - "8000:80"
		     restart: always
		     environment:
		       WORDPRESS_DB_HOST: 
		       WORDPRESS_DB_USER: 
		       WORDPRESS_DB_PASSWORD: 
		       WORDPRESS_DB_NAME: 
		volumes:
		    db_data: {}

### Plan for collaboration ###

- I helped them used GitLab and push their initial code there
- Code from github will be pulled to the front-end container
- Depending on the CS team development process, run a script when needed to dump the DB on their dev machine and import the data to the MySQL container. 




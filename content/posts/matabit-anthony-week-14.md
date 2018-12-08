---
title: "Anthony Week 14"
date: 2018-11-30T17:48:13-08:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 14"]
layout: 'posts'
---
# Week 14
This week's focus was providing services and workshops to the devs. 3/6 of us work at META+LAB and are familiar with the git workflow, so I ran a quick workshop on how to use git for those who were unfamiliar. I also ran a workshop with Docker to remove any confusion with the tool. I also worked on refining the drone CI file to test out deployments. I'm also working on documentation.

## Drone Deployments
For deployment, I had to create a Dockerfile in the dev repo to build application. I used a multi stage build to slim down the image a bit.

```Dockerfile
# Backend
FROM composer:latest as vendor
COPY database/ database/

COPY composer.json composer.json
COPY composer.lock composer.lock

RUN composer install \
    --ignore-platform-reqs \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --prefer-dist

# Frontend
FROM node:8-alpine as frontend

RUN mkdir -p /app/public

COPY package.json webpack.mix.js yarn.lock /app/
COPY resources/ /app/resources/

WORKDIR /app

RUN yarn install \
    && yarn production

# PHP/Apache 
FROM php:7.3-rc-apache

# Install php extensions
RUN docker-php-ext-install pdo_mysql \
    # Enable apache modules
    && a2enmod headers rewrite 

# Add apache.conf file 
COPY ./deploy/apache.conf /etc/apache2/sites-enabled/000-default.conf  

COPY . /var/www/html

# Copy Front/Backend packages
COPY --from=vendor /app/vendor/ /var/www/html/vendor/
COPY --from=frontend /app/public/js/ /var/www/html/public/js/
COPY --from=frontend /app/public/css/ /var/www/html/public/css/
COPY --from=frontend /app/mix-manifest.json /var/www/html/mix-manifest.json

# Change /var/www permission
RUN chown -hR www-data:www-data /var/www

# Expose port 80 and 443
EXPOSE 80 443
```
After I tested out deploying to a docker registry. I used the dockerhub plugin which worked great but I'm having issues with deployments to ECR. It's most likely a config that's causing the issue.

### Drone trigger
I also set a trigger for drone to run on Pull Requests.
```yml
trigger:
  event:
  - pull_request
```

### Other things
I've also created an RDS instance and administered the databases/credentials. After I imported an Excel file into a MySql table.

### Future plans
- Research Secrets manager
- ECR Deployment
- ECS 

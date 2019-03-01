---
title: "Shahid's Blog. Week 21."
date: 2018-3-1T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 21"]
layout: 'posts'
---

## Shahid's Blog. Week 21
#### What's new this week?
This week I had to try and get the new irrigation website up and running. The previous website was using only Laravel but the new website uses Laravel for the backend as well as NodeJS and Angular. I've run into the problem of trying to get two backend software working together at the same time. When looking for a solution online, nothing that I found fixed the issue of the website not utilizing both backends. 

#### What I did instead
I ended up giving up on trying to get both Laravel and NodeJS working together. Instead of wasting more time trying to make the comp group's website work, I spent my time trying to recreate their website using the MEAN stack (MongoDB, ExpressJS, AngularJS, and NodeJS) This is what I have so far.

##### MEAN Stack
- MongoDB
- ExpressJS
- AngularJS
- NodeJS

##### Required
- Curl
- Git
- Apache2
- Firewall
- Text editor

  ```
  sudo apt-get install -y curl git apache2 ufw
  sudo snap install atom --classic
  sudo ufw allow 22/tcp
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo ufw allow 3000/tcp
  sudo ufw allow 4200/tcp  
  ```

##### For servers with a desktop environment
1. Disable screensaver/lock.
  ```
  gsettings set org.gnome.desktop.session idle-delay 0 && \
  gsettings set org.gnome.desktop.screensaver lock-enabled false
  ```

##### Installing and setting up the MEAN Stack
1. Update the repo and packages installed.
  ```
  sudo apt-get update -y && sudo apt-get upgrade -y
  ```

2. Install NodeJS (and NPM)
  ```
  curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
  sudo apt-get install -y nodejs build-essential
  ```

3. Install MongoDB.
  ```
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4

  echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list

  sudo apt-get update -y

  sudo apt-get install -y mongodb-org

  sudo systemctl start mongod

  sudo systemctl enable mongod
  ```

4. Install Angular.
  ```
  sudo npm install -g @angular/cli
  ```

5. Install Express.
  ```
  sudo npm install -g express
  sudo npm install -g express-generator@4.16.0
  sudo npm install -g express-session@1.15.6
  sudo npm install -g @okta/oidc-middleware
  sudo npm install -g @okta/okta-sdk-nodejs
  ```

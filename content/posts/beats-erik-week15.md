---
title: "Erik's Blog. Week 15."
date: 2018-12-07T20:10:32-08:00
tags: ["Erik Blomquist", "BEATS", "Week 15"]
draft: true
---
# SAPS - Dockerize a Node.js application
The CS team decided what they want from us this week. I have started to dockerize a simple node.js app displaying "Hello World" into a Docker image so we can run it as a container.

## Create the Node.js App
1. Create a new folder with a new file called `package.json` in it.
2. Paste the following into your new file and save it.

```
{
  "name": "csun-saps-dev",
  "version": "1.0.0",
  "description": "CSUN SAPS Repository for the application source code",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+ssh://git@gitlab.com/csun-saps-ops/csun-saps-dev.git"
  },
  "keywords": [
    "CSUN",
    "SAPS"
  ],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://gitlab.com/csun-saps-ops/csun-saps-dev/issues"
  },
  "homepage": "https://gitlab.com/csun-saps-ops/csun-saps-dev#readme",
  "dependencies": {
    "express": "^4.16.4"
  }
}

```
3. Use cmd to navigate to your project folder and run `npm install`. This will generate a new file called `package-lock.json` that will be copied into the docker image.

4. Create a new file called `server.js` and paste the following:

```
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello world\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);

```

## Create Dockerfile & .dockerignore
1. In your projects folder, create a new file called `Dockerfile` and paste the following:

```
FROM node:8

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm install --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "npm", "start" ]
```
2. Create a new file called `.dockerignore` and paste the follwowing two lines:

```
node_modules
npm-debug.log
```

## Build & Run image
1. Run `docker build -t korken/saps-web-app .`  
(korken is the name of my personal account, replace it with your username.)
2. It will take some time to install the dependencies, be patient.
3. Once done, run `docker images` and you will see your newly created image there.
4.  Run `docker run -p 49160:8080 -d korken/saps-web-app` which will map port 8080 inside of the container to port 49160 your machine.

## Test
1. Run `docker ps` to see which port you mapped it to.
2. Run `curl -i localhost:49160` or visit http://localhost:49160/.
3. Done! Hello world should be displaying at the bottom.

## Docker commands
- `docker ps` = List containers.
- `docker images` = List images.
- `docker ps -aq` = List all containers.
- `docker stop $(docker ps -aq)` = Stop all running containers.
- `docker rm $(docker ps -aq)` = Remove all containers.
- `docker rmi $(docker images -q)` = Remove all images.

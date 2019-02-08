---
title: "Matabit Shahed Week 18"
date: 2019-02-07T18:28:15-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 18"]
layout: 'posts'
---

# More Documentation...

This week was more about finishing up our infrastructure documentation, and thus making sure that we can pass on this infrastructure to future senior design groups, if necessary.

# Switching from React.js to Angular.js

The Computer Science team has been switching and adding technologies, which is keeping us on our toes. The switch from React.js to Angular.js isn't necessarily a big deal, because we hadn't implemented a "Frontend" container anyways. However, one of our tasks that has been thrown into the sprint for this week is to create a development angular.js container so that the developers can have all their dependencies and dev infrastructure within their docker-compose file. We have managed to include all the containers and change the project structure to accomodate the new container. 
The containers and servers now all start-up, however, the front-end is not properly exposed to the host machine.

Here's our current Angular.js Dockerfile:

```
FROM node:10-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install -g @angular/cli
RUN npm install

COPY . .

EXPOSE 4200
CMD [ "ng", "serve" ]
```

And here's our current project structure for the application repository:
```
.
├── backend
│   ├── config
│   └── node_modules
├── data
│   └── mysql
│       ├── default_database
│       ├── mysql
│       ├── performance_schema
│       └── sys
├── frontend
│   ├── e2e
│   │   └── src
│   ├── node_modules
│   └── src
│       ├── app
│       │   └── components
│       │       ├── highlights
│       │       ├── navbar
│       │       └── splash-image
│       ├── assets
│       │   └── photos
│       └── environments
└── mysql
```

We have a clear separation of responsibilities by having the backend, frontend and the mysql database w/ data volumes being in seperate directories.



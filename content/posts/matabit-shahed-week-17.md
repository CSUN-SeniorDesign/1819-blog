---
title: "Matabit Shahed Week 17"
date: 2019-01-30T08:45:45-08:00
draft: false
tags: ["Shahed Salehian", "Matabit", "Week 17"]
layout: "posts"
---

# Docker Issues & Documentation

This week the computer science team finally got around to start developing. However, they immediately encountered isssues with running the application on their machines, even though Docker is supposed to make portable environments a lot easier. 

They faced two separate issues during this week:
1. Missing `node_modules`
2. Refreshing the application without restarting the server


# node_modules
The first issue was a little bizarre, because at first, I couldn't replicate the issue on my own machine. So I tried cloning the repository one more time into a separate folder and running the `docker-compose` commands again, and lo and behold, I encountered the same issue where the Node.js application could not find any of the packages that were supposed to be installed. 

While troubleshooting, I found out that the `node_modules` were are successfully installed within the container, however, the volume that I have mounted through the `docker-compose` file was overwriting with my local `node_modules` directory.

```YML
...
        volumes:
            - .:/usr/src/app
...
```

So to make sure that the `node_modules` installed within the container are being used we hvae to mount the `node_modules` directory from the container to the host like this:


```YML
...
        volumes:
            - .:/usr/src/app
            - /usr/src/app/node_modules
...
```

# Refreshing the Application

To refresh the application without manually restarting the server a package called `nodemon` is necessary. This package detects changes within the codebase and refreshes as soon as a file with changes has been saved.

To ensure that this works for local development only, we need to add this package as a development dependency and change the startup script for the `docker-compose` file.

Here's how we did that:
### `Package.json`
```JSON
"scripts": {
    "start": "node server.js",
    "dev": "nodemon",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```
Here we add the `"dev"` startup script to simply execute `nodemon`. Nodemon automatically detects which file to use for startup through the `"main"` directive in the `package.json` file

```JSON
  "devDependencies": {
    "nodemon": "^1.18.9"
  }
```

We add `nodemon` under the `devDependencies` so that when `npm install` is run it gets added to the `node_modules`

### `docker-compose.yml`
```YML
        command: npm run dev
```
The `command` directive in the `docker-compose.yml` overwrites the `CMD` directive in the Dockerfile. This way we ensure the correct commands are run in the production and development environment respectively.

# Issues
Some Windows users are reporting issues with the `nodemon` package not working correctly. This still has to the debugged, and might be due to the Docker toolbox on Windows.

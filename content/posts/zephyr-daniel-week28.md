---
title: "Senior Project - Week 28"
date: 2019-04-26T20:37:15-07:00
draft: false
tags: ["Daniel", "Zephyr", "Week 28"]
---

# Week 28
This week we have changed focus and are now working on a new project for our senior design course. We were given three options and of the three we chose the Kubernetes on the spot instances. We decided this option as it has the some technology that I've never used before.

To prepare for the project we have created a new Github public repository where we will be pushing our code for the new project on. A requirement for the repository was that it had to be forked by each member. The only issue is that while I have been using Github for quite some time now I didn't really use it through a command line using git. This wasn't ideal as I shouldn't be using the GUI version of the site but instead and probably more practical to use the command line.

One thing I was confused on doing was forking and how we would push things to the main repository from our fork so that's one thing I learned after setting up the repository.

###### To start we want to fork the github repository we want to work on.
To do that you just go on the Github repository and at the top right of the page should be an option to fork the repository. Doing so will give you a copy of the main repository on your account.

###### Making a local clone to be able to make changes and work with the repository
```
git clone https://github.com/linktoyourrepo
```
The git clone command will clone a repository to your local machine but you do need the link to the repository. Navigating to the repository you just forked you will click the clone or download and copy the https link and past it after the git clone command.

###### Creating a remote to be able to push back to the original repository instead of the fork.
```
git remote add whatevername https://github.com/linktotheoriginalrepo
```
Since we want to make changes to the main branch as well as our fork we need to be able to distinguish between sights when we are pushing anything. In order to do this the git remote command will add a remote so when we push using the "git push origin branch" we can declare something other than origin such as the new remote we are creating. "git remote add upstream link" will need the link of the original or the main repository and upstream will be the name of the remote we are creating.

###### We can now update our repository and push to the origin or the upstream
```
git pull origin master
git push origin master
```
These commands will keep your fork up to date with the main repository

# Tasks as of now
Our group will begin working together on understanding Kubernetes and setting up the infrastructure for it. We are still deciding what we should host on it but getting the infrastructure up and running first is our main priority.

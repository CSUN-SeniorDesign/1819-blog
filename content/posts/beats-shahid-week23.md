---
title: "Shahid's Blog. Week 23."
date: 2018-3-15T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 23"]
layout: 'posts'
---

## Shahid's Blog. Week 23
#### What's new this week?
This week I installed Vagrant in order to create a development environment that is exactly the same as our production environment.

#### Installing Vagrant
1. Go to https://www.vagrantup.com/
2. Click the download button and select the operating system.
3. Install the program with the default settings.
4. Once the installation has finshed, open a terminal or cmder.
5. Type ```vagrant --version``` in order to check that it has installed and that you have the proper version.

#### Setting up Vagrant
##### Creating the workspace
1. Create a folder and name it something like "Vagrant Workspace"

##### Downloading a box
1. Change directory into that folder.
2. Go to https://app.vagrantup.com/boxes/search and choose a virtual machine suited to whatever task you need it for.
3. For a general purpose ubuntu 18.04 machine, run the following:
```
  vagrant init ubuntu/bionic64
```

#### Using Vagrant
1. Open a terminal or cmder.
2. Change directory to the workspace directory you made in the previous step.
3. Run the following to start Vagrant with the default settings found the in Vagrantfile downloaded in the previous step:
```
  vagrant up
```
4. In order to provision the vm using shell, change the following area in the Vagrantfile:
```
config.vm.provision "shell", inline: <<-SHELL
  apt-get update -y
  apt-get install -y apache2
SHELL
```
5. If any changes are made to the vm while the vm is running, use the following command to update the vm.
```
  vagrant reload
```

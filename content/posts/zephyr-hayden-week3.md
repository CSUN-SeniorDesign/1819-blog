---
title: "Project 1 - Post 1: Hayden"
author: "Hayden Wilcoz"
cover: "/img/cover.jpg"
tags: ["Hayden Wilcox", "Zephyr", "Week 3"]
date: 2018-09-13T11:05:21-07:00
draft: false
---
<h3>Hayden Wilcox:</h3><br>

	Assigned Task: Research Ansible and Practice various components and concepts:
			·  Issues arose when attempting to update Oracle Virtual Box, had to delete previous Ubuntu virtual machine and reinstall.
			·  Reinstalled git related commands and re-accessed GitHub to download branches
		o  Due to adding two-factor authentification onto GitHub, I had to learn how to generate a Personal Access Token to allow myself access onto the site via ssh: 
			·   Cloned branch to be used with Ansible 
			·   Setup a personal AWS ubuntu server, using default settings. 
			·   Added the ability for it to be connected via http/s and port :1313 similar to project 1, just in case.
			·   Had difficulties ssh-ing into the instance, attempted to convert .pem file to .ppk as I did in the prior project, but had no success. 
					- .pem file had to be put onto the VM via usb stick, and had to be included in every ssh command.
		o  ssh process was simplified by creating a simple script “ubussh” which contains the line “ssh -i /home/hayden/hayden-test-keys.pem ubuntu@13.58.31.238”
		o  Gave the file execution permission with chmod +x
		o  Doing this made the ssh process easier, as I no longer had to recall the IP or type out the lengthy command, simply ./ubussh
			·   Began following a guide in order to practice using Ansible and use it to setup Nginx. This caused some issues. 
					- It recommended creating a new user to use when performing commands. This proved unnecessary, and also caused a security flag on the .pem file as there was now another user without necessary permissions able to access it. This was solved with chmod +600. 
			·   I used ssh-keygen and added the key to authorized_keys file on the instance, but this proved unnecessary as I did not use the extra user.
			·   Attempted to ping AWS instance, had numerous difficulties. Used the hosts file as my inventory file, but was advised to set up a dynamic inventory file for this purpose later. Research needed. 
			·   In order to ping AWS instance, I needed to install Ansible and Python onto the instance itself. 
					- Initially misunderstood the directions and installed Python onto client VM. 
			·   Python3 was installed on the AWS instance, but the ansible interpreter did not point to it. 
					- Needed to use the command ansible_interpreter_path=/usr/bin/python3 
			·   Was finally able to ping the instance then moved on to attempting to use a playbook to set up nginx. 
			·   Had a few issues with formatting, but these were fixed with a web based formatting software
Made some slight edits to the playbook to fix a connection issue, adding ports etc. but ultimately was unable to get the playbook to work. The screenshot provided is the current error being displayed at the time of writing:

![alt text](https://github.com/CSUN-SeniorDesign/zephyr-blog/blob/master/themes/hugo-lamp/static/img/sh-hayden.png "Screenshot Hayden")
	
Side note: learned a few shortcuts when using vim to make editing files easier and quicker, such as “dG” to clear the entire file, and “u” to undo actions.
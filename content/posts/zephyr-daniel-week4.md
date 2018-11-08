---
title: "Project 1 Post 2: Daniel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 4"]
date: 2018-09-21T21:37:43-07:00
draft: false
---

As mentioned in my first blog post the main thing I was trying to do is establish the Ansible playbook so that it was capable of configuring the server and then deploy it automatically. In order to do the task I needed to familiarize myself more with AWS so that I would be able to properly translate what I think are the actions needed into an Ansible playbook. After setting up an EC2 instance I began looking into the Ansible playbook.

To do the Web server setup we would have to install Nginx into the appropriate instance. The playbook for this was relatively simple.

~~~
---
- hosts: webserver
  become: yes
  tasks:
    - name: Installs Nginx web server
      apt: pkg=nginx state=installed update_cache=true
      notify:
        - start nginx
  handlers:
    - name: start Nginx
      service: name=nginx state=started
~~~

Ansible works using ".yaml" files which uses a language that is easy to read. Within these playbooks you have these parameters that you have set so the playbook knows who to reference and what target you want. From this understanding you begin the playbook by defining a host which would be the target instance you would want to apply this playbook too. In this case "webserver" refers to a tag or group that is found in our Ansible folder. The file it refers to in our current case is our "host" file which contains a group called "webserver" that has listed the instance with its ip and the user I want to join in as, ubuntu. While we are SSh'd into the instance we want to be able to do the things we want so we use "become" which lets us have privilege while in the target location. At this point we are running into issues where the playbook isn't being allowed in using SSH.

In order to troubleshoot this issue we tried to analyze the error we received "WARNING: unprotected private key file" which was an easy fix. We had to go to where the key was located and change the permission using the command sudo chmod 0600 /path/to/key.

The second issue we receive was failed public key authentication. Now at the time this worked on my instance because I was able to go in and put in the public key in the authorized key file and then SSH in but this wasn't as simple. We had many instances running and we were running Ansible from inside the bastion instance trying to SSH into our webserver instance. Our group members were stumped with this issue and another one along side it that was prevented people from accessing our instances through the bastion using regular SSH. Both issues seemed to resolve using a common .pem file that we had to put in the terraform config file. Doing so created instances with a common authentication method and allowed for people to SSH in using a .pem file fixing both issues from my understanding.

After being able to instance into the webserver from the bastion with Ansible we ran the playbook running into our final issue. The playbook wasn’t able to run properly on the target because it seemed to have been missing python requirements. Looking into it we had two solutions we could work with, one short term and one long term. If we wanted to test our terraform portion it would basically destroy the instances and remake them potentially running into the same issue. In order to address this we would add a pre task to our playbook that checks and installs python if it was missing before running the rest of the tasks. The short term solution was to manually install python to check if the playbook would run. We ended up first doing the short term goal and seeing if the nginx installed on the instance, which it successfully did! 

The second task we needed to do was to deploy our server which is something I was very unfamiliar with and had a hard time finding a solution for. It's hard to admit but I did spend more hours than I would have looking for how to do this as easily as I could and was unsuccessful with finding a solution. I had discussed it with my peers and they suggested running a bash script that basically used did the Hugo work and the zipped the files need and then Ansible would grab that and deploy. Discussing it with everyone this seemed like a good solution and we created a bash script that did that and the last Ansible playbook that would grab what was created and push the files onto the instance. After creating the playbook and script we tested it out and ran into a issue.

We had realized that our files for our blog all had a config that would point to our local host instead of the domain URL. Meaning that it wouldn’t show up on our domain unless we changed that, so we had to recreated and edit all the files to show our current domain permanently. We then had to change the script to target those modified files instead. We could still edit and create blogs normally, nothing really changed except that one variable. After words it seemed to work without issue.

During the Ansible research I also helped a little with the route53 portion of the Terraform. I had to add an alias entry to the Route53 using the DNS name of the application load balancer.

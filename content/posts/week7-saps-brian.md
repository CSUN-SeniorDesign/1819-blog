---
title: "Brian's Blog. Week 7 Spring"
date: 2019-8-1T15:57:18-07:00
draft: false
layout: 'posts'
tags: ["Brian Sandoval", "saps", "week7"]
---
# This Week's overview
## Status Update
So far this week was a bit of a disrupture in our workflow apart from being charged twenty dollars for ECS we have decided to use ec2 instead. We have as well as ran into the issue that we will no longer be working with the IS team and have decided along with the computer science team that we will end up going public with our project.
## Future Plans: Datadog
We were confused on which monitoring to use we have are deciding between prometheus or datadog since they seem like the most efficient options. I have been looking into ways of how we can setup and use datadog primarily since I have a bit of experience working with it since I was on it last semester. So far I am trying to recreate some of the things that were done last semester such as setting up a monitor, timeboard, and screenboard.
## Ansible 
My team might end up implmenting more playbooks in the future for the assignment that was due we ended up creating 2 different playbooks the first was a back up of the sudoers and the second playbook was installing a lampstack server using apache, mysql, and php. Below is an examlple of the second playbook came out at the end. The first name tag installs all the packages of what we need. Next it ends up starting the apache service. Lastly, it ends up installing composer using php -r flag from the link below and ends up installing it then finally removes the compose-setup.php file.
```
---
- hosts: localhost
  tasks:
   - name: install lampstack
     become: yes
     become_user: root
     apt:
       pkg:
         - apache2
         - mysql-server
         - php7.2
       state: present
       update_cache: yes
   - name: start apache service
     become: yes
     become_user: root
     service:
       name: apache2
       state: started
       enabled: yes
   - name: Install Composer
     shell: |
       php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
       sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
       rm composer-setup.php
...
```
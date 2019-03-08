---
title: "Irrigation Post - Week 22"
date: 2019-03-08T10:27:31-08:00
draft: false
tags: ["Daniel", "Zephyr", "Week 22"]
---

# Week 22
This week I worked on the Ansible project and did more reading for CicleCI. I'm going to be trying to implement what I know about CircleCI this weekend as I didn't have time during the week. For Ansible I learned about playbooks and how syntax is very important. How ansible uses yaml files for the playbooks too run. I've used YAML files before for thinks like Docker compose and Lambda so the syntax I'm quite familiar with. I know that spacing is important and how you should avoid the use of tabs. Ansible was fun too learn, for this project we had to setup users, create groups and edit the sudoers file to give the groups elevated privileges. To tackle the first issue I had looked up modules for Ansible and two modules stood out with dealing with user management. The group module and user module, the group module allowed me to create a group and name it whatever I needed it to be.

```
- name: Add Trees groups
  group:
    name: Trees
    state: present
```
This snippet is a task for the playbook that adds the group "Trees". Afterwards I added the two users using the user module and added them to the group.
```
- name: Add First user
  user:
    name: Bobby
    group: Trees
```
Repeat this for the second user or I learned that you can be a bit more efficient by introducing lists where you can, in this case, add all the users with just one task instead of creating individual tasks for each user.

After adding the Users and adding them to the group the last thing to do was to edit the sudoers file to include the new group and give them elevated privileges. A concern when I was reading about modifying the sudoers was that it would require special priveleges to modify. Fortunately using the lineinfile module didn't really need anything special. I assume it didn't get an error because adding the line "become: true" gives the playbook elevated priveleges.
```
- name: Changing Sudoers
     lineinfile:
       path: /etc/sudoers
       state: present
       line: '%trees ALL=(ALL) NOPASSWD: ALL'
```
The lineinfile module looks for the file to modify using the path which in this case is the  "/etc/sudoers" and uses the line command to add the following to it. In this case we want to add trees group and give it all the powers like sudo.

The next file that we needed to create was a playbook that created a LAMP stack. This playbook at first glance seemed straight forward cause all I would need to call is many apt-get commands to install each part of the playbook. The only complication would be when installing the composer that would need some dependencies and would need to be installed a different way rather than the apt-get install way.
Installing packages on ansible uses the apt module and you just have to state what you want to install and it will look for it.
```
- name: Install apache2
       apt:
         name: apache2
         state: present
```
I repeated this for each part of the LAMP stack, php, mysql-client and curl for the composer dependencies. In order to install Composer I looked up that you had to get the installer manually. One of the first solutions that I had was using the module get_url that Ansible had but the issue was my understanding lead me that I needed to install the composer specifically to /usr/local/bin for it too work properly. I couldn't figure out how to do that with the get_url module. So I settled with using the shell module that allowed me to execute shell commands and manually manually install it.
```
- name: Install composer
       shell: curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
       args:
         creates: /usr/local/bin/composer
```
The last line lets me put the composer in the location I needed. Running the playbook proved to be successful, I had a check to see if composer ran correctly and the last thing was to restart Apache2 and everything worked as intended. This exercise was really fun and educational and I enjoyed seeing something work as intended.

# Tasks as of Now
So I have created two Docker compose files that install a LAMP stack for what we needed before. The second one installs a MEAN stack for what we need now since we are working on developing the website using our own means we are seeing what works right now. I need to start up the CircleCI and start setting up the CI/CD for the project so whenever something is pushed it will check the code and push our stuff to AWS.

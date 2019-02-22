---
title: "Hat Aubrey Week20"
date: 2019-02-21T20:47:23-08:00
draft: false
---

The branch that i made for this sprint has a working wordpress and sql.

----

Brief Summary:
ECS will create 2 task definitions and 2 services.
The service has new feature called service discovery
I am creating a private route 53 dns zone
registering the ip of the sql there
this url (db-discovery.hat.local) will always point to the ip of the db container because of this
I also enabled dns resolution and dns support in our vpc. 
I also updated the dockerfile to download the latest version of wordpress. I am comparing the download to the sha provided by wordpress in their website.

____________________________________________________________________________

Improvements:
persist the data volume of mysql
add https listener to terraform
certificate is already in the terraform, (it just needs to be uncommented)
setup entrypoint and environment variable for wordpress so we dont have to type the initial part
setup ci/cd to build the wordpress and mysql images and push to the repo
setup lambda to adjust the task def and service (i already have this code. only minor changes needs to be made)
----

Troubleshooting (shouldnt happen anymore):
If tasks/container fail to run: not enough cpu or memory on the ecs instance. not enough ENI on the ecs instance and then there is no other instance
This happened to me when I tried to bring up both contianers in us-west-2a. I only had 1 ECS instance in us-west-2a because the other one is in us-west-2b. What i did was to bring up both ECS instance in us-west-2a and then i was able to bring up the wordpress and mysql container in the same availability zone.
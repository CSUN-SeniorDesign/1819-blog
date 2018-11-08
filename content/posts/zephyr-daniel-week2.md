---
title: "Second Post - Daniel Hirahoka"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 2"]
date: 2018-09-05T18:53:39Z
draft: false
---
<br>
The second week of Project Zero consisted of finalizing and finishing what needed to be done to begin the final tasks of documentation and the presentation. I went forth and did route 53 for the project. To setup the domain I had to log into the AWS IAM account and then find where the Route 53 option was. After finding the Route 53 section in services I had to create a Hosted Zone that would be linked to our Domain that we had purchased from Godaddy. Doing so it gave me two records that were Name Services and Start of Authority, I didn’t know what Start of Authority was so had to look it up. SOA refers to Start of authority and it is a record that identifies the base DNS information about the domain, in this case zephyr90.com. Using the NS record that AWS had given me I logged back into the Godaddy account and replaced the NS section with the AWS version. Afterwards I had gone back into the Route 53 in order to create records for aliases for the domain as well as the requirements needed such as “www.” And “blog.” That would point to our elastic IP provided by the instance we had created. After this was done I went on to help with the TLS certificate, where we had to go to the website Let’s Encrypt which redirected us to the CertBot. Following the instructions provided by CertBot we were able to secure our domain and it’s aliases with a simple command in our SSH. After we had to wait some time before our domain would show that it was secure. Using a third-party website we were also able to confirm that they in fact were secure. Setting up the TLS certificates was not that difficult of a task and was fun to do. Coming to the end of our project we then sat down and started to begin our documentation of the work we had done as well as started to put together our powerpoint. With the power point almost finish we were discussing how we would go about presenting.


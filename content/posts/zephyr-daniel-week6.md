---
title: "Project-2 Post 2 - Daniel"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 6"]
date: 2018-10-05T15:11:21-07:00
draft: false
---

For the second week I was tasked with creating the Cron job that will be used in order to deploy our website. I was tasked with creating the cron job and helping Tonny with creating the script that will be used to grab our files from the s3 bucket and then push the files to our website in order to keep it up to date. The cron job would deploy the script every 5 minutes to keep our website up to date. Creating the cron job is very simple, since the only requirement for the cron job was every 5 minutes the cron job would look like this:

*/5 * * * * /the/path/to/script

This cron job will essentially run every 5 minutes and then run the script. The script that we created was a little difficult to create because we were running into many syntax issues. The issues we were running into had to do when we were trying to compare the tar files we were pulling from the s3 buckets. 

To essentially break down the task of the script, it had to:

1. Pull down the tar file from the s3 bucket
2. Do a sort of comparison to see what is old and what is new.
	a. If there is an update then push it and then move the old one to a new area
	b. If there isnt a new update then just ignore it
Then put this file into the instance that is hosting our website.

First task we had to use the command:

aws 23 cp s3://pathtothefile/ /path/to/target/ --recursive

This command allowed us to copy the tar file from our s3 bucket that would contain our website files whether they are up-to-date or not. Then would push it to a target location that would let us use it.

We then created an if statement that did a comparison of what was in the directory and what we had just grabbed from the bucket. If this comparison came gave a result of a difference then we would replace what was in the target directory and move out what was in there to a directory that would hold old version of the website.

After this it would then grab that tarfile and unzip it into the html folder of our website which will update our website.

After helping with the script I tried assisting the Datadog section of the assignment. I breifly looked over the requirements. I then proceeded to use a method that I don't think was the correct way to do Datadog. We have attempted two methods, one was using Datadog agents on our instances but we weren't able to successfully do it this way. It was thursday night and I decided to try it using the Datadog web services module. They had documentation that would use an AWS role linked to Datadog that would get logs and send them to Datadog.

So the first part was creating the AWS role and Policies that would be associated with the Datadog role, this would allow only reading abilities to the logs and would be able to edit or mess with anything. This is giving them the least permission to them and still allowing them to do what it needs. After creating the policies and roles we would proceed to create a lambda function that would grab the logs and sends them to Datadog. This was documented by Datadog and we used code there code where all we had to do was edit the API key section to have it connect to our Datadog account. Afterwords going back to the Datadog we are now getting logs from our AWS instances from any region.

We are capable of getting metrics from any region and any instance and see everything that was requested from the Project 2. Unfortunately reading further this doesn't seem to be the correct way to do it. While it does achieve the same thing we are missing a certain aspect and that is Datadog-Agent. This is something that needed to be in packer which it is and then go on the instances. While they are on the instances they aren't communicating with our Datadog and essentially not sending any logs. This was something I overlooked personally which makes what I created not really relevant to the Project. While it does achieve the same thing it's not how it was meant to be done.

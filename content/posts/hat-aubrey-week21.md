---
title: "Hat Aubrey Week21"
date: 2019-03-01T09:50:04-08:00
draft: false
---

In regards to our source code and wordpress installation. I was researching and doing some experiments. Here is what I found.
For the sake of clarity:
users == wordpress users that upload and create content
admin == us
developers == developers
visitors == external visitors access the website to consume the content



- Wordpress files served by the webserver have different folders.
- Because wordpress is a CMS and most typically used for blogging, a power user will be constantly creating posts.
- In the wordpress CMS, we, admins, need to create user account for our users. These users are the ones who will create contents and blogposts regularly. 
- Developers are in charged of creating the themes.
- Each post will be broken down to its content and saved in to the database. When someone tries to visit a published post, wordpress will dynamically generate the webpage using the themes that is currently used and data from the database.
- When a user create a post and upload a media, like picture, video or audio, it will go to the uploads folder. The media will not be saved on to the database.
- Developers will primarily only be modifying with the themes folder.
- Wordpress uses "nonce" to secure the application. One of its function is to prevent cross-site forgery. There are about 8 secrets.

		AUTH_KEY
		SECURE_AUTH_KEY
		LOGGED_IN_KEY
		NONCE_KEY
		AUTH_SALT
		SECURE_AUTH_SALT
		LOGGED_IN_SALT
		NONCE_SALT



- Besides specifying the database name and credentials, we need to generate these keys as part of the configuration file so that we don't have to keep entering the information. These keys are 60 characters and they do not have to remain the same everytime. I mean that when a container gets deleted and recreated, these keys could have a different value. The effect on the user is that they will need to relogin to the CMS. Because the container gets deleted, they need to relogin anyway.

## With these backgrounds, I came up with these: ##



- Uploads folder will be in the EFS to make sure that when a user uploads an image, it will be available on all containers
- Themes folder will not be in the EFS so that we can easily do CI/CD on the codes that developers are working on
- Includes and other folder may or may not be on the EFS. Because, they will not be changed as much by the developers, i dont know the best course of actions yet.
- We need to install w3 total cache as a plugin. These will allow us to cache our asset to s3. This way our wordpress environment will only have 2 storage locations. Either EFS or local.
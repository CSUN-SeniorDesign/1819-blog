---
title: "Project5 Post Tonny"
author: "Tonny Wong"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 10"]
date: 2018-11-02T21:54:48-07:00
draft: false
---

Tonny Wong:

	We have worked on implementing Project 4 into Project 5. First we have done it manually to see if everything is working:
	
		- Route 53
		- Certificates
		- CloudFront
		- S3 Bucket
	
	So far we have had an issue with Route 53 where the websites was having 403 error and found out that we had the Alias incorrectly.
	
		- We have redirected the alias towards the S3 bucket and the website was running by doing it manually.
		
	Now we work on the automatic side of the project using terraform to setup everything for us without touching AWS website.
	
		- New feature to deal with was cloudfront and redirecting towards s3 bucket which in certainty had a few complications.
	

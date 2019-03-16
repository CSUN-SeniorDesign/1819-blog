---
title: "Hat Aubrey Week23"
date: 2019-03-15T17:13:10-07:00
draft: false
---
## NFS vs S3 for WordPress ##

Professor Smith tasked our group to use S3 instead of NFS to store the user-uploaded media (pictures, audio, video, etc). 

*Background:*  
- Wordpress stores the user-uploaded media in uploads folder under wp-content folder.
- We mounted the NFS mount point in the /wp-content/uploads in order for all of our wordpress front end container to have the same state (media-wise). 

Why entertain S3?  
- NFS mount point needs to be mounted. It means that at some point it may not be mounted. This adds a layer of complexity and something that may go wrong on website.
- NFS is scalable but not as scalable as S3
- S3 already has built-in versioning, encryption and scalability
- In the long run, s3 may come out cheaper and more reliable

Based on my research, there is no native way to mount s3 as a folder the same way that we did for NFS. Our options as i see it:  
- we can change where wordpress uploads the folder into s3.  
- We can install a plugin that will copy the content of our uploads folder to s3

We are going with the second option. Humanmade.com created a nice plugin that can copy the content of the uploads folder to s3. It will also handle the re-write of the url so that our wordpress site will reference the s3 url when serving the images.

https://github.com/humanmade/S3-Uploads/releases




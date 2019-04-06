---
title: "Zephyr Neel Week 25"
date: 2019-04-5T
draft: false
---
<h3> Neel Patel: </h3>

	Assigned duties for this week:

		- For this week of the Project LAMP I was working on fixing the issue I had with Elastic Load balancer where in the past 
		I was using the availability zones which gave me an error stating that I do not have default VPC, but later through research 
		I found that since I was putting that Elastic Load Balancer in my VPC I created I needed to use the subnets instead of the availability zones. 
		After fixing this error I ran terraform plan to see that everything checks out and things are working perfectly. 
		Also one thing I skipped in Elastic load balancer was the adding the port 443 (HTTPS) using the AWS certification since 
		I do not have any knowledge as of now and had told my groupmate to look into it. After that I create a S3 buckets which will probably 
		in future have files that my private EC2 instances will pull from to display. Also after I created the S3 bucket I needed an AWS IAM role for the Ec2 instances, 
		next I created an IAM policy using terraform to give access to S3 buckets with set permission of what can be done to S3 bucket. 
		After that I attached the policy to the role which will be used to create the IAM instance profile to bind the role to the policy. 
		Now that I have the instance profile I can use it when I launch my linux servers on my private subnets which will add the acess to 
		S3 bucket when my instances are created. After this I am going to test run my ansible script which I created in past sprint to install LAMP and Laravel 
		on my remote private server from my NAT/Bastion server which I on my public subnet with public IP.
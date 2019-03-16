---
title: "Hat Aubrey Week22"
date: 2019-03-08T17:51:57-08:00
draft: false
---
## How EFS and CI/CD work together ##

-Refer to the repository for the code referenced in this post  

- Dockerfile changes:
	- Copy docker-entrypoint.sh to image
	- Enabled Entrypoint in the image
	- chown the theme folder to have the correct permission
- Docker-entrypoint.sh
	- line 107: chown the uploads folder. 
		- At ECS instance creation it will mount EFS volume
		- task def will mount the /efs/ from ECS instance to docker container /var/www/html/wp-content/uploads
		- docker-entrypoint will make sure that the permission is set on the folder properly
	- Our image does not have wp-config.php, the docker-entrypoint.sh will generate this file
		- copy the wp-config-sample.php to wp-config.php
		- If environment variables have been passed, use them to replace the value in the copied wp-config.php
		- This will ensure that our image in the repo does not contain sensitive information
		- However our task definition does, we can take care of it later
		- DevOps/AWS/TerraformUsingModules/src/ecs/main.tf
			- line 119 to 186
	- Also has a line "sed..." to remove any unnecessary "windows"-based encoding but doesnt seem to work properly. 
	- For now when you download this file on your local machine and you have windows, you need to change the Encoding to UTF 8 and change the EoL to Linux (you can use notepad++)
- EFS 
	- create efs and mount point
	- each mount point will generate a dns name to use for mounting it. 
		- DNS resolution must be enabled in VPC. I enabled this last spring for servicediscovery so we are good there
		- Both DNS name will resolve to appropriate mount point based on your Availability zone. So, our ECS instance can have the same value for mount point and they will use the mount point in their AZ.  
	
	Variables Passed to Module
	- num_subnets - var.private_cidr_blocks 
		- used by efs.tf and vpc.tf 
		- Instead of writing the private subnet directly in vpc.tf, I created a variable
		- This will allow me to use the list variable for both efs.tf and vpc.tf
		- I need a list variable because I couldnt use the private subnet output of the vpc.tf to calculate how many mount point to create
		- This is because, when i do length(module.output) , terraform cannot interpolate values that it doesnt already have in its environment state.
		- so, using length(var.subnet) works well. I avoid duplicating writing the list of private subnets.
	- vpc-id - use the output of the module vpc
	- subnets - use the output of the module vpc. 
		- i can use this just fine because I am using this to specify the subnet to create the mount point. 
		- Terraform does not complain because i dont need to interpolate the value using length() 
	- name - name of the EFS
- ecs.tf
- Added new variables to pass: 
- rds_host : output of the module rds
- efs_fqdn : output of the module rds
- security_groups.tf
- added port 2049 on pvt instance as outbound to allow nfs communication
- docker-compose.yml
- just added more environment variables for wordpress docker
- ECS customization
- DevOps/AWS/TerraformUsingModules/src/ecs/main.tf 
	- line 14-21
	- line 41
- DevOps/AWS/TerraformUsingModules/src/ecs/templates/userdata.cfg
	- this is the template that i import on line 14-21
	- This will mount the nfs and register the ecs instance to the cluster at instance creation time
	- it uses cloud-init function baked into the AMI-optimized ecs
- more description later
- EFS mount from EC2 instance to docker
- DevOps/AWS/TerraformUsingModules/src/ecs/main.tf
	- line 96-99
	- line 111-116
- Removed mysql container configuration (task def, service, service discovery, image, repo)
- DevOps/AWS/TerraformUsingModules/src/ecs/mysqlcontainer.tf - i placed it here and commented out
- DevOps/AWS/TerraformUsingModules/variables.tf
- line 23-30
- This is the change that I made for the EFS module to be able to use length() to count how many mount point to create
---
title: "Hat Aubrey Week18"
date: 2019-02-08T05:06:45-08:00
draft: false
---

# Bootstrap Remote Back-end of Terraform #

Using Terraform in a team environment requires a common remote back-end. We chose AWS S3. We went thru different phases on how to use S3 backend.

###Manual Creation###
Initially, we manually created the S3 bucket, and dynamoDB. After this, we initialize our terraform environment using this backend. 

###Local then Migrate###
Another way we did it, is to use a local backend to create the S3. After which, we added the code to use remote backend. All is well. We have S3 in the terraform code. 

However, by doing this, it prevents us from doing terraform destroy without destroying the backend. A quick fix is to put all the related backend codes in a separate folder. This way it will create a separate terraform environment for it. We will end up with multiple terraform.tfstate files. It is a fine solution as long as we create a documentation to comment out the code to use backend when initializing the code in an empty AWS account. 

###Terraform Workspace###
To increase portability of our code, we decided to bootstrap our backend code. The idea is this:

*Folder Structure:*

		base  
		remotestate_setup  
		src

1. Create a workspace "base"
2. Select the workspace "base" (not necessary)
2. Initialize a terraform environment in the remotestate_setup folder
3. In the remotestate_steup folder, call a module from "src" folder to create the remote backend (S3, dynamoDB)
4. Initialize terraform again in the "base" folder to migrate the backend to s3
5. Create a new workspace "production" (Creating a new workspace will automatically switch it also)
6. Do a terraform get to load the other modules in the "src" module

*Additional Comments*

- The base folder contains symlink files so any changes to the original file will reflect to the symlink files.
- The base folder contains:
		
	backend.tf	: symlink from the file in the root folder. It points the terraform backend to S3
	main.tf 	: symlink from the file in the remotestate_setup. It contains the provider setup. It also contains the code to create the module
	variables.tf: symlink from the file in the root folder. Because we have our code in the root folder and it uses the same code, I just symlink it to the variables in the root folder.


From this point forward, we can destroy our environment because we will be working on the "production" workspace and the backend creation is in a different folder. We can also destroy the backend by running the terraform destroy on the base folder

We also put the 7 steps on a script file. When we need to initialize it in a different AWS account, all we have to do is run the script file.  

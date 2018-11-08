---
title: "Project 3 Post 2"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 8"]
date: 2018-10-19T13:15:13-07:00
draft: false
---
<h3> Tonny Wong: </h3>

	Duty this week:
	
		-	Reconfigure CircleCI
		
		-	Work on Lambda

	Duties done this week:
		
		-	Changed some configurations and begun by creating a copy of the current config.yml file and change it to a different name.
			
			- This allows to keep the old files just incase of any encountering issues.
			
			- Then I copied the repository and placed it on my repository for testing.
		
		- First I changed it so that if any changes were done in master this CircleCI will run these codes.
		
		- In the configurations I have started by installing git, python, aws cli in order to get docker.
			
			- The commands used for installation of Docker is:
					
			if [ "${CIRCLE_BRANCH}" == "master" ]; then
                sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
                sudo apt-get update
                sudo apt-get install -y docker-ce
                sudo usermod -a -G docker ubuntu
                docker info
            fi
			
		- After having the it done installing I was verifying how our blog would be placed n a image docker so I have worked with docker with Daniel.
		
			- We had to move the files of the blog into another folder because of the folder name caps.
			
				- (Docker did not like Caps in folders)
			
			- Then we build the docker image for our blog.
			
		- Then I worked with Neel to figure out the Push for ECR
			
			- We required to login into our region with:
				
				eval $(aws ecr get-login --no-include-email --region us-west-2)
				
			- Then it would be possible to push the docker image to the ECR
			
				- It requires it to be tagged and then we can push the image.
			
				
						


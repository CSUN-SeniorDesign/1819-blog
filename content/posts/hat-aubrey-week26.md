---
title: "Hat Aubrey Week26"
date: 2019-04-12T19:08:47-07:00
draft: false
---

# Managing Secrets in AWS ECS #

There are many options to manage the secrets in AWS ECS. This blog will compare and contrast two different ways of using AWS SSM Parameter Store. For the purpose of this blog, secret will refer to sensitive information that needs to be passed to the application such as username, password, database name, etc.
In both options, instead of passing the value of the secret we will pass the name of the key in the parameter store.

One requirement for both of these options, is to put the value in this format to make it easier to inject the value.
WORDPRESS_ADMIN_PASSWORD=aljsdf03asd

**Option 1**: Using AWS Task Definition Secrets Parameter

- In this option we would use the secrets [] parameter in the task definition to pass the name of the key in the parameter store.
- This option requires a execution_role_arn to be defined 
- AWS ecs_agent will handle the retrieval of the secret from the parameter store. 
- Using the role assigned, it will also decrypt the value using the kms key. It is important that the appropriate role is given.

		"memory": 450,
	    "cpu": 500,
	    "secrets": [
	      {
	        "name": "WORDPRESS_ADMIN_USER",
	        "valuefrom": "/Prod/wordpress/wpadminuser"
	      },
	      {
	        "name": "WORDPRESS_DB_USER",
	        "valueFrom": "/Prod/wordpress/wpdbuser"
	      }
	    ],
	    "environment": [
	      {
	        "name": "WORDPRESS_SITE_URL",
	        "value": "https://www.legalforms.info"
	      },

From this example, we can see, that the secrets [] uses valueFrom. The regular environment [] uses value. 

**Option 2**: Using script in the docker entrypoint  

- This is essentially the same as option 1.
- Instead of the ecs_agent doing the retrieval and decryption, the script inside the container will handle it.
- Environment variable can still the same as the insecure way but instead of specifying the actual password, the name of the key in the parameter store will be specified
- The container must still have the correct role to be able to retrieve the key from parameter store and the also decrypt it.
- Custom script must be written to handle it. 
- A sample logic could be:
	- pass the value of the key in parameter store
	- using this value, retrieve the key in the parameter with decryption
	- After that, we can export the value as an environment variable. 
	
		
			"environment": [
		      {
		        "name" : "SSM_SECRETS",
        		"value": "/Prod/wordpress/wpdbpassword /Prod/wordpress/wpauthkey /Prod/wordpress/wpadminpassword"
		      }, 
	 Docker script

			if [[ -n $SSM_SECRETS ]]
				then
				    secrets=$(aws ssm get-parameters --region "us-west-2" --names $SSM_SECRETS --query 'Parameters[*].[Value]' --output text --with-decryption)
				    for secret in "${secrets[@]}"
				    do
				        export $secret
				    done
			fi

This script can take multiple values if necessary.


###Checking for leaks###
For option 1, the only leak is in docker inspect command. For this, a person must have access to the docker host itself.

			Docker inspect
	[
    {
        "Id": "72c76822d3b9f7ac2d871d8b7cd9d5036957b95ce5407c5b28495676aff1129d",
        "Created": "2019-04-09T02:30:35.750733512Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "apache2-foreground"
       
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "SSM_SECRETS=/Prod/wordpress/wpdbpassword /Prod/wordpress/wpauthkey /Prod/wordpress/wpadminpassword", ##Option 2 Value
                "WORDPRESS_DB_DATABASE=wordpress",
                "WORDPRESS_SITE_DESCRIPTION=just wordpress site",
                "WORDPRESS_ADMIN_USER=admin", #Option 1 value
               


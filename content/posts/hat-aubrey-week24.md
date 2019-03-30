---
title: "Hat Aubrey Week24"
date: 2019-03-29T18:28:09-07:00
draft: false
---

# Making Plugin and Theme works #
One of the things that we have put on hold is copying the themes to the image. My task this week is to make plugins work. This is a good time to make the themes work also.


On the CI/CD of gitlab, we checkout our repository. So the runner, will have a copy of everything in the repo. This means that if we do this right, the dockerfile in building image locally (for development purposes) and on CI/CD can and should be the same. 

I need to work on the dockerfile and the docker-entrypoint. This means that I am dealing with the pesky EOL conversion of windows. I set my IDE to use Linux EOL but git is converting them to CRLF (windows). The following command will stop git for windows in converting the script EOL when pushing to the remote repo. 

git config --global core.autocrlf input

Now that's out of the way, here is the breakdown of what I need to do:
1. copy the themes and plugins folder in the image
2. Set the correct permission
3. Configure the plugins
4. Active the plugins


Details:  

Dockerfile 
 
		1. ADD --chown=www-data:www-data ./plugins /var/www/html/wp-content/plugins #copies the plugins folder in the repo to the image and set the correct permission
		2. ADD --chown=www-data:www-data ./theme /var/www/html/wp-content/themes/my-theme #same as above but for themes folder


Docker-entrypoint.sh  

- copy the plugins folder in childsafelegal/Dev/wordpress/plugins
- all folders in plugis directory will be copied to the image
- activate the plugin by adding the wp activate command in the entrypoint
- To write extra stuff in wp-config.php
- pass environment variable called WORDPRESS_CONFIG_EXTRA
- the entire content of this variable will be written as is in the wp-config.php
- each container instance will have the plugin installed and activated by the combination of Dockerfile and Docker-entrypoint.sh
	
  		mkdir -p /var/www/html/wp-content/uploads #creates the uploads folder manually so a consistent behavior can be expected
        chown "$user:$group" /var/www/html/wp-content/uploads
		    uniqueEnvs=(
        AUTH_KEY
        SECURE_AUTH_KEY
        LOGGED_IN_KEY
        NONCE_KEY
        AUTH_SALT
        SECURE_AUTH_SALT
        LOGGED_IN_SALT
        NONCE_SALT
	    )
	    envs=(
	        WORDPRESS_DB_HOST
	        WORDPRESS_DB_USER
	        WORDPRESS_DB_PASSWORD
	        WORDPRESS_DB_NAME
	        "${uniqueEnvs[@]/#/WORDPRESS_}"
	        WORDPRESS_TABLE_PREFIX
	        WORDPRESS_DEBUG
	        WORDPRESS_CONFIG_EXTRA #added this as a way to add more stuff in the wp-config.php
	    )
	    haveConfig=
	    for e in "${envs[@]}"; do
	        file_env "$e"
	        if [ -z "$haveConfig" ] && [ -n "${!e}" ]; then
	            haveConfig=1
	        fi
	    done
		......
		###this part uses php to print the content of WORDPRESS_CONFIG_EXTRA to the file
		 if [ ! -e wp-config.php ]; then
           			awk '
				/^\/\*.*stop editing.*\*\/$/ && c == 0 {
					c = 1
					system("cat")
					if (ENVIRON["WORDPRESS_CONFIG_EXTRA"]) {
						print "// WORDPRESS_CONFIG_EXTRA"
						print ENVIRON["WORDPRESS_CONFIG_EXTRA"] "\n"
					}
				}
				{ print }
			' wp-config-sample.php > wp-config.php <<'EOPHP'	
		.....
		wp plugin activate S3-Uploads --allow-root #activates plugin 
    	wp plugin activate w3-total-cache --allow-root #activates plugin


In our docker-compose.yml, the wordpress_config_extra contains the needed value to configure the s3-upload plugin

		WORDPRESS_CONFIG_EXTRA: |
        /* S3 Upload */
        define( 'S3_UPLOADS_BUCKET', 'my-bucket' );
        define( 'S3_UPLOADS_KEY', 'test' );
        define( 'S3_UPLOADS_SECRET', 'test' );
        define( 'S3_UPLOADS_REGION', 'test' ); // the s3 bucket region (excluding the rest of the URL)



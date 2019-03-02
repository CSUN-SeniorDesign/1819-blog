---
title: "Zephyr Neel Week 21"
date: 2019-02-28T
draft: false
---
<h3> Neel Patel: </h3>

	Assigned duties for this week:
	
			
			- Install LAMP server with Laravel using Ansible script.
				
			
			-For this week of the LAMP stack project my task was to set up an Ansible script that would install 
			LAMP stack with Laravel. As stated in previous blog i do not have any experience with Ansible and its
			my first time using it to deploy a full LAMP stack. Last semester my group mates had created an Ansible script
			that downloaded aws cli and other stuff needed for the project at that time. So to get my self familiar myself with 
			Ansible i started with the most basic project which was display Hello World on my test second Ubuntu virtual machine.
			For this i had to makes sure that both virtual machine were in bridge network so that they would be assigned ip addresses
			so i can deploy the Ansible script. Since i am not familiar with Ansible i started installing stuff one by one and deploy 
			the script after each success. So first i downloaded apache2 on the second test virtual machine and displayed the localhost page 
			using curl.After that i downloaded the mysql-client and php7.1 for now and tested to see the script worked. Then i added the
			php7.1 modules that were needed based on my manual installation and ran the script again. Next to test that php in installed successfully 
			i removed the index.html, which is default page for apache2 and added a phpinfo.php file from my first virtual machine to so i can display the php
			information. At this point i have been successful and so far this week i gotten to downloading the composer using the Ansible script. Next week i am
			going to be starting a laravel project using the composer and hope it goes smoothly with out any errors like i had when i was installing
			manually. Also after i finish i am going to help my group mates with Terraform so we can test the Ansible script on our EC2 instance for proof for the
			presentation.
			
			
			
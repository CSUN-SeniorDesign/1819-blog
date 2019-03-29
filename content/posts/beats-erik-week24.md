---
title: "Erik's Blog Week 24."
date: 2019-03-29T13:38:36-07:00
draft: false
layout: 'posts'
tags: ["Erik Blomquist", "BEATS", "Week 24"]
---

## How To Install Grafana on your Amazon Linux EC2 Instance
Grafana is an open source platform for analytics and monitoring. I am going to show you how to get started with Grafana by simply installing the Grafana agent on our EC2 instance that is running Amazon Linux.

1. First step is to ssh into to your instance. (Create a new instance running Amazon Linux if you haven't done that yet!).

        ssh -i ~/PathToKey.pem ec2-user@PUBLIC-IP

        Eg. ssh -i ~/Downloads/grafana-test.pem ec2-user@34.210.63.1

2. Once logged in to your instance, it's time to download and install grafana.

        sudo yum install -y https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-5.0.3-1.x86_64.rpm
        sudo service grafana-server start /sbin/chkconfig --add grafana-server

3. Make sure that port 3000 allows traffic to your instance.
4. We need to configure our grafana settings. 

         sudo vi /etc/grafana/grafana.ini

5. Scroll down to the server settings and add your public IP to  `http_addr = YOUR IP` and `http_port = 3000`

        #################################### Server ####################################
        [server]
        # Protocol (http, https, socket)
        ;protocol = http

        # The ip address to bind to, empty will bind to all interfaces
        ;http_addr = 34.210.63.1

        # The http port  to use
        ;http_port = 3000

6. Once the configuration has been saved we can restart our grafana server by running: 

        sudo service grafana-server restart

7. You are now ready to see if things work! Head over to http://PublicIP:3000 and you should see the Grafana login page.

        Eg. 34.210.63.1:3000

8. Login with
- Username: admin
- Password: admin

(Best practice is to change the password after your first login.)

9. Once logged in, you need to add a new data source. I used CloudWatch for testing purposes. Click on Add New Data Source and fill out the following:
- Name: **AWS-Grafana-Test**
- Type: **CloudWatch**
- Auth Provider: **Access & Secret key**
- Access key ID: **YOUR ACCESS KEY**
- Secret access key: **YOUR SECRET KEY**
- Region: Select the region you use for your instance. I used **us-west-2**

10. Click Save & Test!

11. Next step is to add a dashboard. Click Create -> Dashboard and select AWS-Grafana-Test as the data source.
12. Select AWS/EC2 in the namespace field.
13. Select which type of metric you want to use for the `select metric` field. Eg. CPUUtilization
14. Select `InstanceID` and then select which instance you want to monitor for the Dimension field.
15. Voila! You should now have a graph that shows the CPUUtilization of your EC2 instance.
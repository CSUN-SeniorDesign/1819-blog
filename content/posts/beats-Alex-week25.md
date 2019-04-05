---
title: "Alex's Blog Week 25."
date: 2019-04-5T14:00:00-07:00
draft: false
layout: 'posts'
tags: ["Alex Enaceanu", "BEATS", "Week 25"]
---
# CIT 481 - SAPS
#### Week 25
For week 25 we have continued to focus on getting our monitoring solutions working together: Prometheus with Grafana. Grafana acts as the dashboard where the metrics are displayed. Prometheus is added to the Grafana setup as a data source. I will cover that below. (Grafana can take data from many sources) First you must install Grafana which is pretty straightforward. in my case - RPM based Linux:

    $ sudo yum install Grafana

Of course we must start the service after installation:

    $ sudo service grafana-server start

You should really configure Grafana to start at boot time since we want the agent running all the time so that it may do its job:

    sudo systemctl enable grafana-server.service


The following files can be found here for your editing pleasure:

ENV files

    /etc/sysconfig/grafana-server

Log files

      /var/log/grafana

Database

    /var/lib/grafana/grafana.db

## Adding data sources
To use prometheus with Grafana, you first have to add the data source to the Grafana interface. You must also have the Admin role added to the Grafana interface as well. After you have done so, click the side menu open by clicking the Grafana icon. Then you would select **data sources** from underneath the **Dashboards** link. Click **+ add data source**. From the drop down menu, select **Prometheus**

Once you select your data source you can then configure the options for that data source. Options are straightforward descriptors associated with your data source. Here are the basics:

* Name (data source name)
* Default (Pre selects data source when new panels are created)
* URL - (http protocol, IP and port info usually 9090 for Prometheus)
* Access - URL access from the Grafana backend
* Basic Auth - Enable basic authentication to Prometheus data sources
* User - Name of Prometheus user
* Password - Database user's Password
* Scrape interval - Lower limit for step query parameters

Make sure you save your data source!

## Creating a Prometheus graph

Click your graph and change the title to whatever you desire. Under **metrics** select your Prometheus data source. Then you enter your Prometheus expression in the Query field and you can see your existing queries show up as you type them. Tune all graph settings to your needs and you should have a working graph!

There are also a multitude of other graphing options for your data source. Check out the graph mode and edit some queries to see the effects. This concludes our brief soiree into Grafana. Stray tuned for more next week!

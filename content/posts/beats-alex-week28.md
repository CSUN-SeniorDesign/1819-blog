---
title: "Alex's Blog Week 28."
date: 2019-04-26T19:01:00-07:00
draft: false
layout: 'posts'
tags: ["Alex Enaceanu", "BEATS", "Week 28"]
---
# CIT 481
#### Week 28
For week 28, we will be exploring the world of bots that operate in a chat app and are capable of running infrastructure commands. First I will be experimenting with building simple apps in Slack through their API.

In this simple example we will only need to ensure that curl is installed on our system. It is available for all platforms so its up to the reader to install the correct version for their trials. Creating an app is quite simple, you sign in to your slack app and navigate to this page:

      https://api.slack.com/apps?new_app=1

The app creation page provides a simple wizard for creating apps. You start by selecting a name for your app. Choose your Slack team from the drop down, then tell choose which channel you want your app to post in and authorize it. You now have an app but it does nothing! To make it do something, select _add features and functionality_. From there, select _incoming webhooks_. These incoming webhooks are URLS created by slack that your app sends its messages to. Slack then forwards that message to the predefined channel or team.

Select add new webhook and then select the channel you want it to post to. After that you can copy the URL created for you which we will use later in a terminal window with the curl command:

      curl -X POST -H 'Content-type: application/json' --data '{"text":"Allow me to reintroduce myself!"}' YOUR_WEBHOOK_URL

Replace YOUR_WEBHOOK_URL with well, your webhook URL. You can of course change the text between the quotes to customize your message. Run that command in a terminal and you should see your app posting in your Slack Team channel that you designated! There are much more robust options for Slack API app building. Especially interesting and useful will be /commands and integrating stackstorm into our Slack bot project.

I created a quick /command that open the AWS management console page. From the app creation menu, select /commands. Specify the name of the / command (in my case /AWS), the URL you wish to request, and a short description. Save your changes and then test your new slash command by typing it in your slack channel. It should open your webpage!

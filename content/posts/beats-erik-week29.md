---
title: "Erik Blog Week 29."
date: 2019-05-03T15:40:36-07:00
draft: false
layout: 'posts'
tags: ["Erik Blomquist", "BEATS", "Week 29"]
---

## Setup a ChatOps Bot for Slack using StackStorm and Hubot.
My previous post demonstrated how to setup the infrastructure for our ChatOps Bot. This post will walk you through the necessary setup to get your bot up and running.

- [Setup Slack](#setup-slack)
- [Setup Hubot](#setup-hubot)
- [Configure StackStorm](#configure-stackstorm)
- [Invite Bot](#invite-bot)
- [Install Pack](#install-pack)


### Setup Slack
First step is to prepare Slack by to creating the bot user itself so that it can communicate with our code.

1. Visit https://api.slack.com/
2. Click ``Start Building``
3. Fill out a proper ``App Name`` and then select your workspace under ``Development Slack Workspace`` and then ``Create App``.
4. Click ``Bots``.
5. Click ``Add a Bot User``.
6. Click ``OAuth & Permissions`` and then ``Install App to Workspace``.
7. Select your workspace and click ``Authorize``.
8. **IMPORTANT!** Copy the ``Bot User OAuth Access Token`` and make sure you do not upload it to the public anywhere.

### Setup Hubot
We need to integrate Hubot to our workspace in Slack.

1. Go to your Slack workspace where you want to implement the bot and click ``Apps`` in the bottom left corner (or the plus sign).
2. In the search field, type ``hubot``and click on the only showing result.
3. You should be re-directed to a slack wegpage. Click ``Install``.
4. Add a Username and then click ``Add Hubot Integration``.

### Configure StackStorm
1. SSH into your instance and create an API key for StackStorm. Run the following command and copy the output.

        st2 apikey create -k

2. We need to configure Stackstorm.

        sudo vi /opt/stackstorm/chatops/st2chatops.env

3. Uncomment the following variables and make sure to replace them with your own values.

        export HUBOT_NAME=fat-lou 
        export HUBOT_ALIAS='!' 

        export ST2_API_KEY="INSERT-YOUR-API-KEY-GENERATED-IN-STEP-1"
        export ST2_AUTH_USERNAME="st2admin" 
        export ST2_AUTH_PASSWORD="testp" 

        export HUBOT_ADAPTER="slack" 
        export HUBOT_SLACK_TOKEN="xoxb-YOUR-HUBOT_SLACK_TOKEN" 

4. Reload the StackStorm configuration to apply our new changes and then start the service.

        sudo st2ctl reload
        sudo service st2chatops start

### Invite Bot
Our bot should now be connected to Hubot/Slack and StackStorm if done properly. We can now invite our by running the following command inside your Slack workspace (Replace fat-lou with the name of your bot).

        /invite @fat-lou 

Check to see if the ``!help`` command works in your workspace. If it does not work, go back to your cmd of your instance and run sudo apt-get ``install -y st2chatops`` and then reload the service.
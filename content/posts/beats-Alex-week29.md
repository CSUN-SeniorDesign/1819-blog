---
title: "Alex Blog Week 29."
date: 2019-05-03T20:40:36-07:00
draft: false
layout: 'posts'
tags: ["Alex Enaceanu", "BEATS", "Week 29"]
---
# CIT 481
#### Week 29
For week 29 our chatops quest continues. We have chosen Hubot as the bot we will implement. For this blog, we will focus on the innerworkings of packs. Packs are a way to deploy integrations and automations via StackStorm. Packs are typically intrinsic to a particular products or services, like AWS and Docker. Packs are managed with this command:

      st2 pack <pack name here>

Packs have their own directory:

      /opt/stackstorm/packs/<mypack>/

You can discover pre-made packs by visiting

   exchange.stackstorm.org

or by searching the pack index from the CLI or with

    st2 pack search

and

    st2 pack show:

Packs are comprised of various utilities known as workflows, rules, sensors, and especially actions and aliases. I will focus on the last two since they are the most important to what we are trying to achieve.

-**Actions** Code snippets that are capable of performing automation, deployments and remediation tasks within your environment. The beauty of actions in StackStorm is that they can be written in any language! Some examples: sending notifications, starting a Docker container or simply restarting a service, among many others. You can run actions from the CLI like such:

      st2 action <your command here>

In order to write custom actions, you must compose a YAML file that describes the action and any inputs. You will also need a script which implements your desired outcome such as the actions described earlier. Below is an example of an action that I used to get familiar with StackStorm and alter for my own purposes:

      # walkthrough/actions/hello_world.yaml
      ---
      name: hello_world
      runner_type: "run-local"
      description: "Example 'Hello, World!' action"
      enabled: true
      parameters:
        cmd:
          default: "echo Hello, World!"

-**Aliases** are a way to abbreviate actions into a human readable form that is well suited towards text based interfaces such as chatops!  Aliases are written in YAML and are deployed via packs since they considered content just like actions, sensors and rules. Below is an example action alias that executes a remote code:

      name: "remote_shell_cmd"
      pack: "examples"
      action_ref: "core.remote"
      description: "Execute a command on a remote host via SSH."
      formats:
      - "run {{cmd}} on {{hosts}}"

-**Workflows** are a combination of actions. Workflows link actions together to automate and orchestrate your infrastructure needs. They can be deployed across multiple systems. To create a workflow, you must choose a workflow runner and then connect the desired actions together in a workflow definition file.

-**Sensors** are a convenient way to integrate infrastructure that is external to yours into your StackStorm deployment. Sensors will check systems periodically as defined by you for certain events. They can also wait for incoming events.  Sensors are written in Python but they also require a YAML file.

-**Rules** are used for mapping triggers (which are like sensors except that they identify an inbound event) to actions. Rules too, are defined in YAML as seen below in the example of a rule's definition structure.

      name: "rule_name"                      # required
      pack: "examples"                       # optional
      description: "Rule description."       # optional
      enabled: true                          # required

      trigger:                               # required
          type: "trigger_type_ref"

      criteria:                              # optional
          trigger.payload_parameter_name1:
              type: "regex"
              pattern : "^value$"
          trigger.payload_parameter_name2:
              type: "iequals"
              pattern : "watchevent"

      action:                                # required
          ref: "action_ref"
          parameters:                        # optional
              foo: "bar"
              baz: "{{ trigger.payload_parameter_1 }}"

As you can see, all of these components work in tandem within a pack to achieve your infrastructure goals. We have been developing our own pack customized with a few of our own actions that we created for the project. One of my actions I have been writing \ testing, is a traceroute script:

      #!/usr/bin/env bash

      HOST=$1
      MAX_HOPS=$2
      MAX_QUERIES_TO_HOP=$3

      MAX_HOPS="${MAX_HOPS:-30}"
      MAX_QUERIES_TO_HOP="${MAX_QUERIES_TO_HOP:-3}"

      # echo "$HOST"
      # echo "$MAX_HOPS"
      # echo "$MAX_QUERIES_TO_HOP"

      TRACEROUTE=`which traceroute`
      if [ $? -ne 0 ]; then
          echo "Unable to find traceroute binary in PATH" >&2
          exit 2
      fi

      exec ${TRACEROUTE} -q ${MAX_QUERIES_TO_HOP} -m ${MAX_HOPS} ${HOST}

The issue is that I am having issues passing arguments to the slash command. For example in my traceroute script I can enter an IP address or hostname to run the trace against. I am just having issues getting it running in the slack app.

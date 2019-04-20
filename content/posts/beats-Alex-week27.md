---
title: "Alex's Blog Week 27."
date: 2019-04-19T22:01:00-07:00
draft: false
layout: 'posts'
tags: ["Alex Enaceanu", "BEATS", "Week 27"]
---
# CIT 481 - SAPS
#### Week 27
Our project is coming to an end, and one of the goals to have a successful environment before we wrap things up, is to implement monitoring. We have had a few different monitoring options that we tried (DataDog, Prometheus) each had unique challenges which forced us to look elsewhere. I have documented how to install and add Grafana data sources to the Grafana dashboard in a previous blog (week 25) so I will focus on how to install and configure Grafana with a Ansible which is good timing since I have been assigned the following tasks for week 27:

  - Ansible playbook that installs and configures Grafana
  - Grafana Dashboard

Here is the playbook I am testing and modifying install-grafana-playbook.yml broken down:

As you can see below, the first portion of the playbook specifies the variables which are the desired application, architecture, version and filename information.

        - hosts: HOST
          vars:
              grafana_version: 6.1.4
              arch: amd64
              grafana_filename: grafana_{{ grafana_version }}_{{ arch }}.deb


The tasks section of the playbook is going to verify the grafana version by utilizing the variables from the top of the playbook, and dpkg-query to check the dpkg database for any existing grafana installations. The change_when line does the comparison.

          tasks:
            - name: Verify grafana version
              command: dpkg-query -W --showformat='${version}' grafana
              register: grafana_check_version
              failed_when: grafana_check_version.rc > 1
              changed_when: (grafana_check_version.rc == 1) or
                          (grafana_check_version | version_compare("{{ grafana_version }}", "<"))

Downloads the file from the specified url and specifies the desired destination when the there is a version update resultant from the comparison above:

        - name: Download grafana file
          get_url:
              url="https://grafanarel.s3.amazonaws.com/builds/{{ grafana_filename }}"
              dest="/tmp/{{ grafana_filename }}"
              when: grafana_check_version.changed

        - name: Verify grafana file
              command: dpkg-deb -W --showformat='${version}' /tmp/{{ grafana_filename }}
              register: grafana_deb_check_version
              failed_when: grafana_deb_check_version.stdout != "{{ grafana_version }}"
              when: grafana_check_version.changed


  Below is the part where Grafana is installed if all verifications above pass:
          - name: Install grafana
            apt: deb="/tmp/{{ grafana_filename }}" state=present
        when: grafana_check_version.changed

You can check the output of your playbook to see if any changes were made:  


        PLAY RECAP
        ************************************************************************************************************************************
        : ok=4    changed=1    unreachable=0    failed=0

  You can also run this command in a terminal to verify installation:

              grafana --version

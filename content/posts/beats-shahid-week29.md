---
title: "Shahid's Blog. Week 29."
date: 2019-05-02T12:17:21-07:00
layout: 'posts'
tags: ["Shahid Karim", "EAT", "Week 29"]
---

## Shahid's Blog. Week 29
#### What's new this week?
#### Working with kubernetes
This week I was working on getting Kubernetes working on a local vm then applying this knowledge towards getting it working on an EC2 or spot instance.

### Question #1: minikube start command states "Unable to start VM"
- These are the causes of this issue:
    You might be trying to start minikube in a local VM:

    1. This is due to virtualbox not being installed on the machine. 

    2. VT-X/AMD-v is not enabled to allow nested VMs.

    #    
    #### Answer
    
    - This is fixed by doing the following:
        1. Install virtualbox by running the following command:
        ```
        sudo apt-get install virtualbox
        ```
        2. If you are running the VM in vmware then click on VM > Settings > Processors > Check Virtualize Intel VT-x/EPT or AMD-V/RVI then restart the VM.

#### Install VirtualBox or VMWare
1. Run the following command:
    ```
    sudo apt-get install virtualbox
    ```

2. Make sure that Intel VT-x/EPT or AMD-V/RVI is enabled.


#### Install Kubectl
1. Run the following commands:
    ```
    sudo apt-get update && sudo apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update
    sudo apt-get install -y kubectl
    ```

#### Install Minikube
1. Run the following commands:
    ```
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    ```

2. Make sure Intel VT-x/EPT or AMD-V/RVI is enabled.

#### Install Docker
1. Run the following command:
    ```
    sudo apt-get install docker
    ```


#### Run Minikube
1. Run this command to start Minikube
    ```
    minikube start
    ```

2. Run these commands to check that the cluster is running:
    ```
    kubectl get nodes
    kubectl cluster-info
    ```

3. Start the minikube dashbaord:
    ```
    minikube dashboard
    ```
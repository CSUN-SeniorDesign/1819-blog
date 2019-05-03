---
title: "Senior Project - Week 29"
date: 2019-05-03T10:34:52-07:00
draft: false
tags: ["Daniel", "Zephyr", "Week 29"]
---

# Week  29
This week we have declared our new project and have begun working on it. Our project consists of setting up a Kubernetes cluster on Spot instances. Afterwards if we have time we are going to try to implement our website into our Kubernetes cluster and set it up, this requires setting up a MEAN stack using Docker. Since we are just starting our first order of business is to setup the Kubernetes Cluster, then put set it up on spot instances then if we have time the Docker files with the MEAN stack implementation.

I started by setting up a Kubernetes Cluster using Minikube.

###### Issues
I wanted to address a couple of issues that I ran into when trying to setup the Kubernetes cluster using Minikube.

###### 1. VT-X/AMD-V not Enabled
During the setup of the cluster and after running Minikube it got to the step where it was setting up the virtual environment using virtual box. At the time I was using Virtualbox with Ubuntu 18.04 which was running Minikube and another Virtualbox installation.

```
Virtual Box Ubuntu 18.04
|___Minikube
    |___VirtualBox
```
After checking to see if VT-X/AMD-V was enabled on all virtual boxes and checking to see if it was running on the Guest machine it seemed everything should have worked. Unfortunately Virtualbox doesn't support nested virtualization so I had to change the main vm I was using. I changed to Vmware workstation Pro and I didn't encounter this error anymore. There were some memory issues and processor issues but that was because the Guest OS was setup with certain processors and memory that was being exceeded by the Virtual box inside. It essentially didn't build because it didn't have enough resources which was mitigated by increases the resources for the Guest OS.
```
VMware Workstation pro Ubuntu 18.04
|___Minikube
    |___VirtualBox
```
###### 2. APIServer Proxy
Minor issue I encountered was while it was setting up the Cluster and Minikubes got to the section where it was setting up the proxies for the pods it would crash. Giving me an issue with the APIServer Proxy, the short term solution was to run Minikube with sudo but that brought issues in the next step when setting up the Dashboard. I assumed it was a permissions issue which it might be. I didn't find a good solution for this issue because the next time I ran Minikubes it worked without sudo. I did run:
```
kubectl get pods --all-namespace
```
Which just gave me a list of pods that have been setup but I don't think it fixed the issue.

#### Setting Up Kubernetes using Minikube on Ubuntu 18.04
If you're going to be setting up using this method you will want to use a VM that supports nesting virtualization such as VMware Workstation Pro.

My environment consisted of using a windows machine running VMware Workstation Pro running Ubuntu 18.04 that would run Minikube and VirtualBox.
```
Windows 10 (Main Machine)
|__VMware Workstation pro Ubuntu 18.04
    |___Minikube
        |___VirtualBox
```

##### Setting up a cluster
###### 1. Installing Virtualbox
Update the packages and the system to the most up to date version
```
sudo apt update
sudo apt upgrade
```
Then setup the "Apt repository" on your system and we want to setup the "Oracle key" to our system.
```
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
```
Next we want to install the Oracle VirtualBox PPA to our system.
```
sudo add-apt-repository "deb http://download.virtualbox.org/virtualbox/debian bionic contrib"
```
Finally Install the VirtualBox
```
sudo apt Update
sudo apt install virtualbox-6.0
```

###### 2. Installing kubectl

Kubectl is how we are going to interact with our clusters.

To download the latest release of Kubectl
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```
Need to make the kubectl an executable binary
```
chmod +x ./kubectl
```
Then move it to PATH
```
sudo mv ./kubectl /usr/local/bin/kubectl
```
Then we check if it is working properly and we installed the most up to date version
```
kubectl version
```

###### 3. Installing Docker
Docker is used for our containers, or our pods that Minikube will setup in the cluster.

Update your packages
```
sudo apt update
```
Install some prerequisites. These are used to allow "apt" to use packages over https
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Add the GPG key for official Docker to our systems repository
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add Docker repositories
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
Update system packages again.
```
sudo apt update
```
Install Docker
```
sudo apt install docker-ce
```
Then check to see if it is running.
```
sudo systemctl status docker
```

###### 4. Installing Minikube
This program will aid us in our use of local Kubernetes applications development.

Installing is simple
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

###### 5. Running and Configuration
We will be running a simple cluster and we can do this by using minikube and Kubectl
To start up minikube
```
minikube start
```
To get more information about the cluster
```
kubectl get nodes
```

Fortunately for us minikube configures kubectl automatically but if you wanted to determine or change anything in the configuration you can access the config file
```
~/.kube/config
```
We can then access the dashboard if everything was configured correctly by Minikube
```
minikube dashboard
```


# Task as of now
I need to now figure out how to setup the pods for the cluster to use. Then I have to set this up in a spot instance using AWS. Hopefully I can figure this out by this weeked.

---
title: "Hat Aubrey Week29"
date: 2019-05-03T21:42:13-07:00
draft: false
---

# Learning Kubernetes #

## Setup Cluster ##

In this blog, I will talk about setting up a local kubernetes cluster using Vagrant and Ansible.  
I will use Vagrant to bring up the VM that I want and use ansible local provisioner to configure them.  
I will have 1 master and 2 nodes
I am using Hyper-V as my hypervisor, therefore, some of the networking functionality of vagrant needs to be manually inputted during the provisioning phase  

Vagrantfile

	BOX_IMAGE = "bento/ubuntu-18.04" #Use 18.004 as it has stable version of docker
	NODE_COUNT = 2 #used for specifying node counts (node == workers)
	
	Vagrant.configure("2") do |config|
	  #specify password for smb because it is tiring to keep typing it for testing
	  config.vm.synced_folder ".", "/vagrant", type: "smb",
	    smb_password: "", smb_username: "",
	    mount_options: ["username=","password="]
	  config.vm.define "master" do |subconfig|
	    subconfig.vm.box = BOX_IMAGE
	    subconfig.vm.network "public_network"
	    config.vm.provider "hyperv" do |h|
	      h.cpus = 3
	      h.memory = 2048
	    end
	  end
	  (1..NODE_COUNT).each do |i|
	    config.vm.define "node#{i}"  do |subconfig|
	      subconfig.vm.box = BOX_IMAGE
	      subconfig.vm.network "public_network"
	    end
	  end
	  #provision using ansible
	  config.vm.provision "ansible_local" do |ansible|
	    ansible.playbook = "serverconfig.yml"
	  end
	  config.vm.provision "ansible_local" do |ansible|
	    ansible.playbook = "kube.yml"
	  end
	  config.vm.provision "ansible_local" do |ansible|
	    ansible.playbook = "master.yml"
	  end
	end

I have 3 playbooks so far.

	serverconfiguration.yml    
	tasks:
    - name: create the 'ubuntu' user
    - name: allow 'ubuntu' to have passwordless sudo
    - name: set up authorized keys for the ubuntu user
    - name: Disable SWAP since kubernetes can't work with swap enabled (1/2)
    - name: Disable SWAP in fstab since kubernetes can't work with swap enabled (2/2)
    - name: Update and upgrade apt packages
  
kube.yml

	- name: Add Docker GPG key
	 apt_key: url=https://download.docker.com/linux/ubuntu/gpg
	- name: Add Docker APT repository
	 apt_repository:
	  repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ansible_distribution_release}} stable
	- name: Install list of packages
	 apt:
	  name: ['apt-transport-https','ca-certificates','curl','software-properties-common','docker-ce', 'docker-ce-cli', 'containerd.io']
	  state: present
	  update_cache: yes
	- name: Start docker on boot
	 systemd:
	  name: docker
	  state: started
	  enabled: yes
	- name: add Kubernetes apt-key
	 apt_key:
	   url: https://packages.cloud.google.com/apt/doc/apt-key.gpg
	   state: present
	
	- name: add Kubernetes' APT repository
	 apt_repository:
	  repo: deb http://apt.kubernetes.io/ kubernetes-xenial main
	  state: present
	  filename: 'kubernetes'
	
	- name: install kubelet
	 apt:
	   name: kubelet=1.14.0-00
	   state: present
	   update_cache: true
	
	- name: install kubeadm
	 apt:
	   name: kubeadm=1.14.0-00
	   state: present
	
	- hosts: master
	become: yes
	tasks:
	- name: install kubectl
	 apt:
	   name: kubectl=1.14.0-00
	   state: present
	   force: yes

master.yml

	  - name: initialize the cluster
      become: yes
      shell: kubeadm init --pod-network-cidr=10.244.0.0/16 >> cluster_initialized.txt
      args:
        chdir: $HOME
        creates: cluster_initialized.txt

    - name: create .kube directory
      file:
        path: $HOME/.kube
        state: directory
        mode: 0755

    - name: copy admin.conf to user's kube config
      become: yes
      copy:
        src: /etc/kubernetes/admin.conf
        dest: /home/ubuntu/.kube/config
        remote_src: yes
        owner: ubuntu

    - name: install Pod network
      shell: kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml >> pod_network_setup.txt
      args:
        chdir: $HOME
        creates: pod_network_setup.txt
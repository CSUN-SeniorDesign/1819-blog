---
title: "Matabit Anthony Week 24"
date: 2019-03-30T18:38:12-07:00
draft: false
tags: ["Anthony Inlavong", "Matabit", "Week 24"]
layout: 'posts'
---
# Week 24
Due to sprint break and other events, the team has not developed much, however, things started to kick back into gear. On the ops side, nothing exciting happened in terms of infrastructure improvements. I did run into an issue of my ELK server being reassigned because of its spot status. This gave me a chance to test my ELK playbook and everything was still good to do with a single CLI `ansible-playbook --ask-vault-pass elk.yaml elk-load.yaml certbot.yml` I want to get my hands dirty with Kubernetes just to try something new, so my goal this sprint was to create a kubernetes cluster by hand. 

## Kubernetes cluster
So there are various cloud providers that provide a managed kubernetes cluster. This is great! But I wanted to create my own to understand the inner workings. There are many tools to create a cluster such as kubespray, kops, and kubeadm. I chose Kubeadm as it's the officially supported way and many of the blog/Reddit posts recommended good 'ol reliable. Before I even touch the tools I much provision a few servers in AWS. I used terraform + spot instances to create 1 master node, and 2 worker nodes. Kubernetes also requires a few [ports](https://kubernetes.io/docs/setup/independent/install-kubeadm/) to be open, so I had to create two new security groups.

```json
resource "aws_spot_instance_request" "kube" {
  ami                         = "${data.aws_ami.ubuntu_18_latest}"
  spot_price                  = "0.0069"
  instance_type               = "t3.small"
  key_name                    = "${var.key_name}"
  monitoring                  = false
  associate_public_ip_address = true
  count                       = 3
  wait_for_fulfillment        = true
  user_data                   = "${file("../cloud-init.conf")}"
  security_groups             = ["${module.kube_master_sg.this_security_group_id}", "${module.kube_worker_sg.this_security_group_id}"]
  subnet_id                   = "${data.terraform_remote_state.vpc.public_subnet_a}"
  provisioner "local-exec" {
    command = "aws ec2 create-tags --resources ${self.spot_instance_id} --tags Key=Name,Value=kube-${count.index}"
  }
  tags {
    Name = "kube"
  }
}
```

## Tasks    
* Install dependecies
* Initialize the cluster + network with Kubeadm
* Create a .kube directory
* Copy admin.conf into user’s .kube directory
* Install a pod network
  * Calico
  * Flannel
* Create a join token
* Worker
* Join worker to the cluster

### Ansible playbook
I've created Ansible playbooks to configure the base of the cluster. The `kube-dependencies.yaml` file contains roles used to install Docker and kubernetes essentials. The master playbook initializes the cluster, creates a pod network (Calico), and configures the user to manage the master node. The worker playbook creates a join token for the workers to become a part of the cluster. 

#### Master playbook
```yaml
---
- hosts: master
  become: yes
  tasks:
    - name: install kubectl
      apt:
        name: kubectl=1.14.0-00
        state: present
        update_cache: yes
        
    - name: initialize the cluster
      shell: kubeadm init --pod-network-cidr=192.168.0.0/16

    - name: create .kube directory
      become: yes
      become_user: anthony
      file:
        path: $HOME/.kube
        state: directory
        mode: 0755

    - name: copy admin.conf to user's kube config
      copy:
        src: /etc/kubernetes/admin.conf
        dest: /home/anthony/.kube/config
        remote_src: yes
        owner: anthony

    - name: install Pod network
      become: yes
      become_user: anthony
      shell: kubectl apply -f https://docs.projectcalico.org/v3.6/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml >> pod_network_setup.txt
      args:
        chdir: $HOME
        creates: pod_network_setup.txt

    - name: remove taint
      become: yes
      become_user: anthony
      shell: kubectl taint nodes --all node-role.kubernetes.io/master-
```

#### Worker playbook
```yaml
---
- hosts: master
  become: yes
  gather_facts: false
  tasks:
    - name: get join command
      shell: kubeadm token create --print-join-command
      register: join_command_raw

    - name: set join command
      set_fact:
        join_command: "{{ join_command_raw.stdout_lines[0] }}"

    - name: debug master
      debug:
        var: join_command

- hosts: workers
  become: yes
  tasks:
    - name: join cluster
      shell: "{{ hostvars[groups['master'][0]]['join_command'] }} >> node_joined.txt"
      args:
        chdir: $HOME
        creates: node_joined.txt
```
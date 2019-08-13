# Kubernetes Vagrant + Ansible Provisioning

This is to provision CentOS Virtual Machines and setup a Kubernetes cluster using ansible. The created machines are 

1. Control VM with Ansible
2. Kubernetes Master
3. Kubernetes Node 

It spins up 3 VMs in Virtual Box with the following configuration:

* OS: CentOS 7
* Memory: 2048
* CPU: 2

## Pre-Requisites

* Download and Install VirtualBox for your OS: https://www.virtualbox.org/wiki/Downloads

* Download and Install Vagrant for your OS: https://www.vagrantup.com/downloads.html

* Generate a private and public key on your machine on the following location: `~/.ssh/id_ansible` using the following command `ssh-keygen -t rsa -b 4096 -C "ansible"`


## Installation

In order to execute the script run:


```bash
vagrant up
```

## Removal

In order to remove all the 3 VMs created by vagrant run:

```python
vagrant destroy -f
```


# -*- mode: ruby -*-
# vi: set ft=ruby :

FEDORA_IMAGE_NAME = "centos/7"
UBUNTU_IMAGE_NAME = "generic/ubuntu1804"
NODES = 2

Vagrant.configure("2") do |config|
  
  # Box Settings
  # config.vm.box = IMAGE_NAME
  
  # Provider Settings
  config.vm.provider "hyperv" do |h|
    h.memory = 2048
    h.maxmemory = 4096
  end

  config.vm.define "ansible" do |ctl|
      ctl.vm.box = UBUNTU_IMAGE_NAME
      ctl.vm.network "private_network", ip: "192.168.50.9"
      ctl.vm.hostname = "ansible"
      # ctl.vm.provider "virtualbox" do |vb|
      #   vb.memory = 2048
      # end
  end

  config.vm.define "k8s-master" do |master|
    master.vm.box = FEDORA_IMAGE_NAME
    master.vm.network "private_network", ip: "192.168.50.10"
    master.vm.hostname = "k8s-master"
    # master.vm.provision "ansible" do |ansible|
    #     ansible.playbook = "kubernetes-setup/master-playbook.yml"
    #     ansible.extra_vars = {
    #         node_ip: "192.168.50.10",
    #     }
    # end
  end

  (1..NODES).each do |i|
    config.vm.define "k8s-node-#{i}" do |node|
        node.vm.box = FEDORA_IMAGE_NAME
        node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
        node.vm.hostname = "node-#{i}"
        # node.vm.provision "ansible" do |ansible|
        #     ansible.playbook = "kubernetes-setup/node-playbook.yml"
        #     ansible.extra_vars = {
        #         node_ip: "192.168.50.#{i + 10}",
        #     }
        # end
    end
  end

  # Network Settings
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"
  
  # Folder Settings
  # config.vm.synced_folder "../data", "/vagrant_data"
  
  # Provision Settings
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end

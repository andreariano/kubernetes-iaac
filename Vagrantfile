CENTOS_IMAGE_NAME = "centos/7"
NODES = 2

Vagrant.configure("2") do |config|
  # Box Settings
  config.vm.box = CENTOS_IMAGE_NAME

  # SSH settings
  $script = <<-SCRIPT
    cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF
  SCRIPT
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["~/.ssh/id_ansible", "~/.vagrant.d/insecure_private_key"]
  config.vm.provision "file", source: "~/.ssh/id_ansible", destination: "~/.ssh/id_ansible"
  config.vm.provision "file", source: "~/.ssh/id_ansible.pub", destination: "~/.ssh/authorized_keys"
  config.vm.provision "shell", inline: "chmod 0600 /home/vagrant/.ssh/id_ansible"
  config.vm.provision "shell", inline: $script

  # Provider Settings
  config.vm.provider "virtualbox" do |h|
    h.memory = 2048
    h.cpus = 2
  end

  config.vm.define "k8s-master" do |master|
    # VM basic setup
    master.vm.hostname = "k8s-master"
    master.vm.network "private_network", ip: "192.168.50.10"
  end

  config.vm.define "ansible" do |control|
    ## VM basic setup
    control.vm.hostname = "ansible"
    control.vm.network "private_network", ip: "192.168.50.9"

    # Install ansible
    control.vm.provision "shell", inline: "sudo yum install -y ansible"

    # Copy over ansible playbooks
    control.vm.provision "file", source: "./ansible/", destination: "~/"

    # Execute ansible playbook
    #control.vm.provision "shell", inline: "cd /home/vagrant/ansible && ansible-playbook --inventory-file=hosts -v master-playbook.yml --private-key=~/.ssh/id_ansible "
    control.vm.provision "ansible_local" do |ansible|
      # ansible.verbose = "vvvv"
      ansible.limit = "all"
      ansible.playbook = "/home/vagrant/ansible/playbooks/cluster.yml"
      ansible.provisioning_path = "/home/vagrant/ansible/"
      ansible.inventory_path = "/home/vagrant/ansible/hosts"
      ansible.raw_arguments = ['--private-key=/home/vagrant/.ssh/id_ansible']
    end
  end

  # (1..NODES).each do |i|
  #   config.vm.define "k8s-node-#{i}" do |node|
  #       node.vm.box = CENTOS_IMAGE_NAME
  #       # node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
  #       node.vm.hostname = "node-#{i}"
  #       # node.vm.provision "ansible" do |ansible|
  #       #     ansible.playbook = "kubernetes-setup/node-playbook.yml"
  #       #     ansible.extra_vars = {
  #       #         node_ip: "192.168.50.#{i + 10}",
  #       #     }
  #       # end
  #   end
  # end

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

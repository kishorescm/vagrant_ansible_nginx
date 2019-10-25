# -*- mode: ruby -*-
Vagrant.require_version ">= 2.2.6"
NUMBER_OF_WEBSERVERS = 2
CPU = 2
MEMORY = 256
ADMIN_USER = "vagrant"
ADMIN_PASSWORD = "vagrant"
VM_VERSION= "puppetlabs/ubuntu-14.04-64-nocm"
VAGRANT_VM_PROVIDER = "virtualbox"

Vagrant.configure(2) do |config|

  groups = {
    "webservers" => ["web[1:#{NUMBER_OF_WEBSERVERS}]"],
    "loadbalancers" => ["load_balancer"],
    "all_groups:children" => ["webservers","loadbalancers"]
  }

  # create web servers

  (1..NUMBER_OF_WEBSERVERS).each do |i|
    config.vm.define "web#{i}" do |node|
        node.vm.box = VM_VERSION
        node.vm.hostname = "web#{i}"
        node.vm.network :private_network, ip: "10.0.15.2#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        node.vm.provider VAGRANT_VM_PROVIDER do |vb|
          vb.memory = MEMORY
        end

    # Only execute once the Ansible provisioner,
    # when all the machines are up and ready.

      if i == NUMBER_OF_WEBSERVERS
          node.vm.provision "ansible" do |ansible|
            ansible.verbose = "v"
            ansible.playbook = "pb_web.yml"
            ansible.become = "yes"
            ansible.limit = "all"
            ansible.groups = groups
          end
        end

      end
    end


    # create load balancer
    config.vm.define "load_balancer" do |lb_config|
        lb_config.vm.box = VM_VERSION
        lb_config.vm.hostname = "lb"
        lb_config.vm.network :private_network, ip: "10.0.15.11"
        lb_config.vm.network "forwarded_port", guest: 80, host: 8011
        lb_config.vm.provider VAGRANT_VM_PROVIDER do |vb|
          vb.memory = MEMORY
        end
        lb_config.vm.provision "ansible" do |ansible|
          ansible.verbose = "v"
          ansible.playbook = "pb_lb.yml"
          ansible.become = "yes"
          ansible.limit = "all"
          ansible.groups = groups
        end
    end
end

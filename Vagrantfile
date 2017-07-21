# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = "centos/7"
NODE_COUNT = 3

Vagrant.configure("2") do |config|

  config.vm.define "admin", primary: true do |admin|
    admin.vm.box = BOX_IMAGE
    admin.vm.provider "virtualbox" do |vb|
      # Customize the amount of memory on the VM:
      vb.memory = "512"
    end

    admin.vm.hostname = "admin"
    admin.vm.network :private_network, ip: "10.10.0.10"

    admin.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "admin-prepare.yml"
      ansible.verbose  = true
      ansible.install  = true
      ansible.config_file    = "ansible.cfg"
    end

    admin.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.verbose        = true
      ansible.limit          = "all" # or only "nodes" group, etc.
      ansible.inventory_path = "inventory"
      ansible.config_file    = "ansible.cfg"
    end
  end

 (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |nodeconfig|
      nodeconfig.vm.box = BOX_IMAGE
    
      nodeconfig.vm.hostname = "node#{i}"
      nodeconfig.vm.network :private_network, ip: "10.10.0.#{i + 10}"

    end
  end


end

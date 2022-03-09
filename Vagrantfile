# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.define  "VM_Jenkins" do |db|
    db.vm.box = "hashicorp/bionic64"
    db.vm.hostname = "VMJenkis"
    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    db.vm.network "private_network", ip: "192.168.33.10"
    db.vm.network "forwarded_port", guest: 5000, host: 8080
    db.vagrant.plugins = "vagrant-docker-compose"
    # install docker and docker-compose
    db.vm.provision :docker
    db.vm.provision :docker_compose   
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
  
  end

  config.vm.define  "VM_Host" do |h|
    h.vm.box = "hashicorp/bionic64"
    h.vm.hostname = "VMHost"
    h.vm.network "private_network", ip: "192.168.33.11"
    h.vm.network "forwarded_port", guest: 3306, host: 3307
    h.vagrant.plugins = "vagrant-docker-compose"
    # install docker and docker-compose
    h.vm.provision :docker
    h.vm.provision :docker_compose

    h.vm.provider "virtualbox" do |vb1|
      vb1.memory = "1024"
    end
    #h.vm.provision "shell", in line: "echo '127.0.0.1 localhost VMHost\n192.128.33.11 VMJenkins > /etc/hosts"
  end
end
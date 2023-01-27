# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vagrant.plugins = ["vagrant-vbguest", "vagrant-hostmanager"]
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true

  config.vm.box = "ubuntu/bionic64"

  config.vm.synced_folder "./share", "/share"
  
  # always before setting up worker nodes
  config.vm.define "master" do |c|
    c.vm.hostname = "master.internal"
    c.vm.network "private_network", ip: "192.168.56.11"
    c.vm.provider "virtualbox" do |v|
      v.gui = false
      v.cpus = 2
      v.memory = 2048
    end

    c.vm.provision :shell, :path => "scripts/common/setup-initial-ubuntu.sh"
    c.vm.provision :shell, :path => "scripts/common/setup-docker-ubuntu.sh"
    c.vm.provision :shell, :path => "scripts/common/setup-k8s.sh"
    c.vm.provision :shell, :path => "scripts/master/setup-k8s-master.sh"
  end

  (1..2).each do |n|
    config.vm.define "worker-#{n}" do |c|
      c.vm.hostname = "worker-#{n}.internal"
      c.vm.network "private_network", ip: "192.168.56.2#{n}"
      c.vm.provider "virtualbox" do |v|
        v.gui = false
        v.cpus = 1
        v.memory = 2048
      end

      c.vm.provision :shell, :path => "scripts/common/setup-initial-ubuntu.sh"
      c.vm.provision :shell, :path => "scripts/common/setup-docker-ubuntu.sh"
      c.vm.provision :shell, :path => "scripts/common/setup-k8s.sh"
      c.vm.provision :shell, :path => "scripts/worker/setup-k8s-worker.sh"
    end
  end

  config.vm.define "nfs" do |c|
    c.vm.hostname = "test.internal"
    c.vm.network "private_network", ip: "192.168.56.51"
    c.vm.provider "virtualbox" do |v|
      v.gui = false
      v.cpus = 1
      v.memory = 1024
    end

    c.vm.provision :shell, :path => "scripts/common/setup-initial-ubuntu.sh"
  end

end

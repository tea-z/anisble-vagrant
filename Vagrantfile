# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.define :haproxy do |haproxy|
    haproxy.vm.hostname = "haproxy"
    haproxy.vm.box = "bento/centos-7.4"
    haproxy.vm.network :private_network, ip: "10.1.1.40"
    haproxy.vm.synced_folder ".", "/vagrant"
    haproxy.vm.provider "virtualbox" do |vm|
      vm.customize ["modifyvm", :id, "--memory", 1024, "--cpus", 1]
      vm.gui = false
    end
    haproxy.vm.provision "ansible" do |ansible|
        ansible.sudo = true
        ansible.verbose = "v"
        ansible.playbook = "haproxy.yml"
    end
  end
  config.vm.define :nginx1 do |nginx1|
    nginx1.vm.hostname = "nginx1"
    nginx1.vm.box = "bento/centos-7.4"
    nginx1.vm.network :private_network, ip: "10.1.1.41"
    nginx1.vm.synced_folder ".", "/vagrant"
    nginx1.vm.provider "virtualbox" do |vm|
      vm.customize ["modifyvm", :id, "--memory", 1024, "--cpus", 1]
      vm.gui = false
    end
    nginx1.vm.provision "ansible" do |ansible|
        ansible.sudo = true
        ansible.verbose = "v"
        ansible.playbook = "nginx.yml"
    end
  end
  config.vm.define :nginx2 do |nginx2|
    nginx2.vm.hostname = "nginx2"
    nginx2.vm.box = "bento/centos-7.4"
    nginx2.vm.network :private_network, ip: "10.1.1.42"
    nginx2.vm.synced_folder ".", "/vagrant"
    nginx2.vm.provider "virtualbox" do |vm|
      vm.customize ["modifyvm", :id, "--memory", 1024, "--cpus", 1]
      vm.gui = false
    end
    nginx2.vm.provision "ansible" do |ansible|
        ansible.sudo = true
        ansible.verbose = "v"
        ansible.playbook = "nginx.yml"
    end
  end
end

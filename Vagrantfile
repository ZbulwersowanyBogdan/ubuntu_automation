# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

$script = <<-SCRIPT
echo I am provisioning...
sudo apt update -y
sudo apt install ansible -y
echo "192.168.56.11" >> /vagrant/ip_to_ping
SCRIPT

$script2 = <<-SCRIPT
echo kaszaneczka
SCRIPT

Vagrant.configure("2") do |config|

  # Configure the Ubuntu Machines.
  config.vm.box = "ubuntu/focal64"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: false
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--nested-hw-virt", "on"]
  end

  # First one, for installing ansible
  config.vm.define "focal-ansible" do |config|
    config.vm.hostname = "focal"
    config.vm.network :private_network, ip: "192.168.56.10"
    config.vm.provision "shell",
    inline: $script
  end

  # Second one, for installing docker with jenkins container
  config.vm.define "focal-jenkins" do |config|
    config.vm.hostname = "focal"
    config.vm.network :private_network, ip: "192.168.56.11"
    config.vm.network "forwarded_port", guest: 8080, host: 8080
    config.vm.provision "shell",
      inline: $script2

  end
end
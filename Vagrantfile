# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

### Upload config file to VM ###
    config.vm.synced_folder ".", "/home/vagrant/",  type: "rsync"	
#Make it a bit more personal
  config.vm.box = "centos/7"
  config.vm.hostname = "packstack"
###Do not lose time with port-frorwarding just make the instance bridged and enable promiscuous mode after
  config.vm.network 'public_network', type: 'dhcp', auto_config: true
  config.vm.define "packstack" do |packstack|
  end
  
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 4
    vb.memory = "8192"
	vb.name = "OpenStack"
  end

  ## TODO: add rules to allow access to vagrant ports from your local machine
    
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa -q
    sudo bash -c "ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa -q"
    sudo bash -c "umask 077 && cat ~vagrant/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys"

    sudo yum -y install centos-release-openstack-rocky
    sudo yum -y install openstack-packstack
    sudo yum -y install e2fsprogs

    sudo systemctl disable NetworkManager
    sudo systemctl stop NetworkManager
	
##Start the deployment
    packstack --answer-file=answers.txt
  SHELL
end

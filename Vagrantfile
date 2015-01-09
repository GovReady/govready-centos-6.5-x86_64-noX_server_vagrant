# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Configure GovReady Centos 6.5 vm
  config.vm.define "centos65govready" do |centos65govready|
    centos65govready.vm.box = "govready-centos-6.5-x86_64-noX-0.2.1-server-0.8.1"
    centos65govready.vm.box_url = "https://a7240500425256e5d77a-9064bd741f55664f44e550bdad2949f9.ssl.cf5.rackcdn.com/govready-centos-6.5-x86_64-noX-0.2.1-server-0.8.1.box"

    # network config
    centos65govready.vm.network :private_network, ip: "192.168.56.110"
    centos65govready.vm.network :forwarded_port, guest: 80, host: 8083
    centos65govready.vm.network :forwarded_port, id: 'ssh', guest: 22, host: 2322, auto_correct: false,  d: "ssh"

    # Sync overall cloudstart directory on host machine with "/vagrant" directory on guest machine
    centos65govready.vm.synced_folder ".", "/vagrant", group: "vagrant", owner: "vagrant", create: true

    # Launch virtualbox GUI window
    # Set v.gui = false to hide GUI window
    centos65govready.vm.provider "virtualbox" do |v|
      v.gui = true
    end
  end

end
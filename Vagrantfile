# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::configure("2") do |config|

  # Operating System
 
  ## Ubuntu 12.04 (64-bit)
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Boot with a GUI so you can see the screen. (Default is headless)
  # config.vm.boot_mode = :gui

  # Set the default project share to use nfs
  config.vm.synced_folder "./www", "/vagrant/www/", :nfs => true
  config.vm.synced_folder "./db", "/vagrant/db/", :nfs => true

  # Set the Timezone to something useful
  config.vm.provision :shell, :inline => "echo \"America/Chicago\" | sudo tee /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata"  

  # Update the server
  config.vm.provision :shell, :inline => "apt-get update --fix-missing"

  ###
  #  Creates a basic LAMP stack with MySQL
  #  
  #  To launch run: vagrant up mysql
  ###
  config.vm.define :lamp do |lamp_config|
    # Map dev.pyrocms.mysql to this IP
    config.vm.network :private_network, ip: "192.168.50.4"

    # Enable Puppet
    mysql_config.vm.provision :puppet do |puppet|
      puppet.facter = { 
        "fqdn" => "dev.pyrocms.mysql", 
        "hostname" => "www", 
        "docroot" => '/vagrant/www/pyrocms/'
      }
      puppet.manifest_file  = "ubuntu-apache2-mysql-php5.pp"
      puppet.manifests_path = "puppet/manifests"
      puppet.module_path  = "puppet/modules"
    end
  end


end

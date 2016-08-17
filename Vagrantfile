# -*- mode: ruby -*-
# vi: set ft=ruby :

# General project settings
#################################

# IP Address for the host only network, change it to anything you like
# but please keep it within the IPv4 private network range
ip_address = "172.22.22.20"

# The project name is base for directories, hostname and alike
project_name = "project_name"

# Vagrant configuration
#################################

Vagrant.configure("2") do |config|
  # Enable Berkshelf support
  config.berkshelf.enabled = true

  # Use the omnibus installer for the latest Chef installation
  config.omnibus.chef_version = :latest

  # Define VM box to use
  config.vm.box = "ubuntu/trusty64"

  # Set share folder
  config.vm.synced_folder "./" , "/vagrant/", :mount_options => ["dmode=777", "fmode=666"]

  # Use hostonly network with a static IP Address and enable
  # hostmanager so we can have a custom domain for the server
  # by modifying the host machines hosts file
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.vm.define project_name do |node|
    node.vm.hostname = project_name + ".local"
    node.vm.network :private_network, ip: ip_address
    node.hostmanager.aliases = [ "www." + project_name + ".local" ]
  end
  config.vm.provision :hostmanager

  config.vm.provider "virtualbox" do |vb|
     # override virtualbox name
     # vb.name = project_name
  end

  # Enable and configure chef solo
  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "default"
    chef.add_recipe "usability"
    chef.add_recipe "usability::zsh"
    chef.add_recipe "usability::ssh-key"
    chef.json = {
      :default => {

      },
      :usability => {

      }
    }
  end
end

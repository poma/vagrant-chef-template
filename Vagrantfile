project_name = File.basename(__dir__)

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.vm.hostname = project_name
  config.hostmanager.aliases = [ project_name + ".local" ]
  config.vm.network "private_network", type: "dhcp"
  config.vm.define project_name # Specify node name for Vagrant

  #config.ssh.username = 'root'
  #config.ssh.password = 'vagrant'
  #config.ssh.insert_key = 'true'

  config.vm.provision :hostmanager
  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "default"
    chef.add_recipe "usability"
    chef.add_recipe "usability::zsh"
    chef.add_recipe "usability::root"
    chef.add_recipe "usability::ssh-key"
    chef.json = {
      :default => {

      },
      :usability => {

      }
    }
  end

  # swith vbox dhcp to 172.16.192/24 subnet (may trigger UAC)
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natnet1", "172.16.192/24"]
  end

  # fetch machine ip using VBoxManage. ToDo: get rid of absolute path
  config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
	#if hostname = (vm.ssh_info && vm.ssh_info[:host])
    #  `vagrant ssh -c "hostname -I"`.split()[1]
    #end
	if vm.id
	  #`VBoxManage guestproperty get #{vm.id} "/VirtualBox/GuestInfo/Net/1/V4/IP"`.split()[1]
	  #`"%VBOX_MSI_INSTALL_PATH%/VBoxManage.exe" guestproperty get #{vm.id} "/VirtualBox/GuestInfo/Net/1/V4/IP"`.split()[1]
	  `"$VBOX_INSTALL_PATH/VBoxManage" guestproperty get #{vm.id} "/VirtualBox/GuestInfo/Net/1/V4/IP"`.split()[1]
    end
  end
end

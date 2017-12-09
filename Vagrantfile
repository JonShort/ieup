# -*- mode: ruby -*-
# vi: set ft=ruby :

# Box name defaults to win7-ie11, use an env var to use different boxes
box_name = box_name = ENV['box_name'] != nil ? ENV['box_name'].strip : 'win7-ie11'

# Box repo defaults to http://aka.ms (where modern.ie boxes are stored). Set env var to override
box_repo = ENV['box_repo'] != nil ? ENV['box_repo'].strip : 'http://aka.ms'

# Env variable for users choosing to RDP into the vm - toggles whether gui should be displayed by virtualbox
vm_gui = ENV['vm_gui'] == 'false' ? false : true

Vagrant.configure("2") do |config|
  ## Box

  # If the box is win7-ie11, the convention for the box name is modern.ie/win7-ie11
  config.vm.box = "modern.ie/" + box_name

  # If the box is win7-ie11, the convention for the box url is http://aka.ms/vagrant-win7-ie11
  config.vm.box_url = box_repo + "/vagrant-" + box_name

  ## System
  config.vm.guest= :windows
  config.vm.boot_timeout = 500

  ## Shared Folders
  config.vm.synced_folder "./shared", "C:/shared"
  config.vm.synced_folder "./tools", "C:/tools"

  # RDP forward
  config.vm.network "forwarded_port", guest: 3389, host: 3389, id: "rdp", auto_correct: true

  ## Remote Access (WinRM)
  config.vm.communicator = "winrm"
  config.winrm.username = "IEUser"
  config.winrm.password = "Passw0rd!"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = vm_gui
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id,  "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
  end
end

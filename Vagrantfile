# -*- mode: ruby -*-
# vi: set ft=ruby :

require_relative "vmList"
include VMList

# Env Variables
# Override box to use - default is "win7-ie11"
BOX_NAME ||= ENV["BOX_NAME"] != nil ? ENV["BOX_NAME"].strip : "win7-ie11"
# Override box download location - default http://aka.ms/ (where modern.ie boxes are stored).
BOX_REPO ||= ENV["BOX_REPO"] != nil ? ENV["BOX_REPO"].strip : "http://aka.ms/"
# Env variable for users choosing to RDP into the vm - toggles whether gui should be displayed by virtualbox
BOX_GUI ||= ENV["BOX_GUI"] == "false" ? false : true

Vagrant.configure("2") do |config|
  # If the box is win7-ie11, the convention for the box name is modern.ie-win7-ie11
  config.vm.box = "modern.ie-" + BOX_NAME

  # If the box is win7-ie11, the convention for the box url is http://aka.ms/ie11.win7.vagrant
  config.vm.box_url = BOX_REPO + BOXES[BOX_NAME]

  ## System
  config.vm.define BOX_NAME
  config.vm.guest = :windows
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
    vb.gui = BOX_GUI
    vb.name = BOX_NAME
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
  end
end

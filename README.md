# vagrant-windows-config
Simple windows VM config for Vagrant

This is mostly for personal use, but there could be some useful aspects to it for others, so feel free to look around / clone / fork.

I've added answers to questions I had while configuring this [here](#faq)

Huge thanks to [this gist](https://gist.github.com/andreptb/57e388df5e881937e62a) for providing a lot of the info that got winrm working.

# Requirements
**Virtualbox**
Virtualbox is used by vagrant to spin up the VM
- Get Virtualbox [here](https://www.virtualbox.org/)

**Vagrant**
- Get Vagrant [here](https://www.vagrantup.com/)

# Initial setup
- Check the `Vagrantfile` for information on how to choose a specific VM from modern.ie. By default, `win7-ie11` is used
- Run `vagrant up`
- The VM will being downloading if it doesn't already exist (this may take a while)
- Your VM should be running with a GUI
- When prompted, choose 'work' for the network setting within the VM.
- Common vagrant commands can be seen [here](#running-the-vm)
- For information on setting up shared folders, see [here](#accessing-shared-folders)

# Running the VM
- Run `vagrant up` to run the VM within this directory
- Run `vagrant status` to see running VMs
- Run `vagrant halt` to stop the running VM
- Run `vagrant suspend` to stop the running VM, saving the current state (will resume from here on next `vagrant up`)
- Run `vagrant destroy` to remove the VM (useful if the VM needs spinning up from scratch again)

## Accessing shared folders
note - This only needs to be done once, when initialising the VM (e.g. first time using it, or after a `vagrant destroy`)
- Run `vagrant up`
- Once running, open windows start menu and search `local security policy`
- Open **Local Security Policy**
- Go to the **Network List Manager Policies** folder
- Find the **network** entry, and change:
- **Location type**: 'private'
- **User permissions**: 'User can change location'
- Shut down the VM
- Run `vagrant up` again - this time, shared folders will be visible under 'Network'

## Fully configuring winrm
These steps should be done after [accessing shared folders](#accessing-shared-folders)
- If not already running, run `vagrant up`
- Navigate to `Network > tools`
- Run `setup-winrm.bat` as administrator
- Note - all winrm commands should be run as administrator

## Adding chocolately & puppet
These steps should be done after [accessing shared folders](#accessing-shared-folders)
- If not already running, run `vagrant up`
- Navigate to `Network > tools`
- Run `setup-choco.bat` as administrator

# FAQ
### Where are downloaded Vagrant boxes stored?
The `VAGRANT_HOME` env var can be used to configure the location that boxes will be downloaded to, however the default is:
- **Mac OS X and Linux**: ~/.vagrant.d/boxes
- **Windows**: C:/Users/USERNAME/.vagrant.d/boxes
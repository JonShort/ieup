<h1 align="center">
  <img src="https://raw.githubusercontent.com/jonshort/ieup/master/logo.svg" alt="ieup" title="ieup" width="200" style="max-width: 100%;">
  <br>
    ieup
  <br>
</h1>

Spin up IE & Edge VMs with one command

This is mostly for personal use, but there could be some useful aspects to it for others, so feel free to look around / clone / fork.

I've added answers to some common questions around this [here](#faq)

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
- The VM will begin downloading if it doesn't already exist (this may take a while)
- Your VM should be running with a GUI
- When prompted, choose 'work' for the network setting within the VM.
- Common vagrant commands can be seen [here](#running-the-vm)
- For information on setting up shared folders, see [here](#accessing-shared-folders)

# Running the VM
- Run `vagrant up` to run the VM currently selected
- Run `vagrant status` to see running VMs
- Run `vagrant halt` to stop the running VM
- Run `vagrant suspend` to stop the running VM, saving the current state (will resume from here on next `vagrant up`)
- Run `vagrant destroy` to remove the VM (useful if the VM needs spinning up from scratch again)

# Choosing a VM
A list of available VMs can be viewed in `./vmList.rb`. These can be used be setting the `BOX_NAME` env var to one of the 'keys' (the strings on the left of the list).
- Check the correct one is available / running with the `vagrant status` command

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

### How do I RDP into a running box?
Once a box is running & connected (you may need to follow the [fully configuring winrm](#fully-configuring-winrm) steps), you can access the box by:
- Run `vagrant rdp`
- This is supposed to bring up the default rdp client, but it doesn't work for me, so i just grab the credentials and manually add the box/s to my client.

### How do I remove a vagrant box instance (but keep the downloaded box)?
Sometimes it's useful to start from scratch, and delete an instance of a box, without having to download the VM again when spinning it up.

This can be done by:
- Run `vagrant global-status`
- Find the name or ID of the box instance you want to remove
- Run `vagrant destroy [ID/Name]`

### How do I delete a VM i've downloaded?
Boxes can be deleted by:
- Run `vagrant global-status`
- Find the name or ID of the box you want to delete
- Run `vagrant box remove [ID/Name]`

### How do I set environment variables?
Environment variables (env vars) are used in this repo to configure which boxes are downloaded, & how.

**On Mac / Linux**
To set env vars temporarily:
- Run `export [variable name]="variable value"`
- An example could be `export BOX_NAME="win7-ie8"`

To set env vars permanently (reccomended):
- Run `cd ~`
- Find `.bash_profile`
- Add `export [variable name]="variable value"` to the file
- An example could be `export BOX_GUI="false"`

**On Windows**
Unfortunately i've not had to handle env vars on windows, so I can only defer to the official documentation [here](https://msdn.microsoft.com/en-us/library/windows/desktop/ms682653(v=vs.85).aspx)
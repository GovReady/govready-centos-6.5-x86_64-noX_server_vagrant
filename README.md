GovReady CentOS 6.5 64bit Server Baseline
=========================================

Vagrant file to download and launch a virtual machine of CentOS 6.5 64bit headless vagrant box pre-configured to SCAP-Security-Guide RHEL 'server' profile.

# Status

| Date                  | Status                      | Time to Build           |
|-----------------------|-----------------------------|-------------------------|
| Jan 04, 2015 Created  | Build OK. openSCAP OK.      | About 3 minutes or less |


## Requirements
This repo uses Vagrant and VirtualBox.

Requirements:
- VirtualBox (http://virtualbox.org)
- Vagrant (http://vagrantup.com)

## Instructions
After you install VirtualBox and Vagrant, clone this repository and run `vagrant up`.

Your first `vagrant up` will take longer as the vagrant box is downloaded.

Remember to first `cd` into the directory where you want to save this repo!

```
git clone git@github.com:GovReady/govready-centos-6.5-x86_64-noX_server_vagrant.git
cd govready-centos-6.5-x86_64-noX_server_vagrant
vagrant up
```

You can now log into your running CentOS with openSCAP and SSG installed.
```
vagrant ssh
```

# Building from Source

Want to understand how this virtual machine was assembled? All necessary files are included in the `source` subdirectory. 

File                         | Description
-----------------------------|---------------------------------------------------------------------------
source/Vagrantfile           | Vagrantfile to launch CentOS 6.5 testmachine that is not fully locked down
source/CHECKLIST.md          | List steps to semi-manually configure the CentOS vm to the server baseline profile
source/vbkick-template       | VBKick files to build the CentOS 6.5 machine image from source CentOS files


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

You can now log into your running CentOS with GovReady, openSCAP and SCAP-Security-Guide installed.
```
vagrant ssh
```

# GovReady - Check Configuration

Run these commands from your GovReady virtual machine.

You will need to be root to run all tests. On a standard vagrant vm, the root password is `vagrant`.
```
# After you logged into your vm
# You will need to be root to run all tests
su -

cd /home/myfisma
govready scan
```

You should see the results:
```
OpenSCAP NIST Certified SCAP Scanner Results for Profile "server"

The "server" profile identifies 13 high severity controls. OpenSCAP says 12 passing and 1 failing.
The "server" profile identifies 89 medium severity controls. OpenSCAP says 88 passing and 1 failing.
The "server" profile identifies 72 low severity controls. OpenSCAP says 68 passing and 4 failing.

```

### Why are some tests failing?

The few tests that are failing would effect the smooth running of vagrant, for example, setting a boot password.


# Building from Source

Want to understand how this virtual machine was assembled? All necessary files are included in the `source` subdirectory. 

File                         | Description
-----------------------------|---------------------------------------------------------------------------
source/Vagrantfile           | Vagrantfile to launch CentOS 6.5 testmachine that is not fully locked down
source/CHECKLIST.md          | List steps to semi-manually configure the CentOS vm to the server baseline profile
source/vbkick-template       | VBKick files to build the CentOS 6.5 machine image from source CentOS files


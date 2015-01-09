# GovReady CentOS 6.5 checklist

This is the most current instructions to making an accreditation-ready CentOS VM.

Updated 01.04.2015

# Launch VM

Launch GovReady CentOS 6.5 testmachine
```
cd govready-centos-6.5-x86_64-noX_server_vagrant
vagrant up centos65govready
vagrant ssh centos65govready
```

# Install and Configure GovReady
Install GovReady
Note: Will install openSCAP and SCAP-Security-Guide automatically

```
# Install govready using curl. govready will install OpenSCAP and SCAP-Security-Content
curl -Lk io.govready.org/install | sudo bash
```

Configure GovReady
```
# Switch to root so scanner can run all tests properly
su - 

# Create a directory and cd into it
cd /home
mkdir myfisma
cd myfisma

# Initialize the directory
govready init

# Import CentOS cpe-dictionary.xml and cpe-oval.xml SCAP data into local scap/content directory
govready import https://raw.githubusercontent.com/GovReady/govready/xplatform/templates/ssg-centos6-cpe-dictionary.xml
govready import https://raw.githubusercontent.com/GovReady/govready/xplatform/templates/ssg-centos6-cpe-oval.xml

# Update GovReadyfile using sed command (or update the CPE line manually using a text editor)
sed -i 's:^CPE.*:CPE = scap/content/ssg-centos6-cpe-dictionary.xml:' GovReadyfile

# Update profile to server
sed -i 's:^PROFILE.*:PROFILE = server:' GovReadyfile
```

# Scan
Scan 
```
govready scan
```

# Fix 
```
govready fix
```

fix audit.rules
```
curl -Lk https://raw.githubusercontent.com/GovReady/govready/master/templates/audit.rules-rhel6 > audit.rules-rhel6
mv -f audit.rules-rhel6 /etc/audit/audit.rules
service auditd restart
```

fix set_password_hashing_algorithm_logindefs
```
sed -i 's:^ENCRYPT_METHOD.*:ENCRYPT_METHOD SHA512:' /etc/login.defs
```

fix set_password_hashing_algorithm_libuserconf
```
sed -i 's:^crypt_style.*:crypt_style = sha512:' /etc/libuser.conf
```

Update /etc/pam.d/system-auth
```
cp -f /etc/pam.d/system-auth /etc/pam.d/system-auth.orig
curl -Lk https://raw.githubusercontent.com/GovReady/govready/master/templates/system-auth-rhel6 > /etc/pam.d/system-auth
```

Update /etc/pam.d/password-auth
```
cp -f /etc/pam.d/password-auth /etc/pam.d/password-auth.orig
curl -Lk https://raw.githubusercontent.com/GovReady/govready/master/templates/password-auth-rhel6 > /etc/pam.d/password-auth
```

Fix no_shelllogin_for_systemaccounts
Comment: Ensure that System Accounts Do Not Run a Shell Upon Login; Detail: OpenSCAP should report which system accounts (userid < 500) that need to be set to not run shell upon login, excluding the root account. on a virtual machine there is a vboxadd sysaccnt account with user id 498. This account allows login.  
```
# fix
usermod -s /sbin/nologin vboxadd
# test
govready rule no_shelllogin_for_systemaccounts
```

fix sysctl_ipv4_all_send_redirects
Note: Medium. 
Note: To set the runtime status of the net.ipv4.conf.all.send_redirects kernel parameter, run the following command:
```
sysctl -w net.ipv4.conf.all.send_redirects=0
```
Note: If this is not the system's default value, add the following line to /etc/sysctl.conf:
```
net.ipv4.conf.all.send_redirects = 0
```

fix set_iptables_default_rule
```
sed -i 's/^:INPUT.*/:INPUT DROP [0:0]/' /etc/sysconfig/iptables
```

fix enable_auditd_bootloader
Note: best idea is to check /etc/grub.conf file first before doing this command
```
sed -i 's/quiet$/quiet audit=1/g' /etc/grub.conf
```

fix set_system_login_banner
Note: correct carriage returns in `/etc/issue`
```
curl -Lk https://raw.githubusercontent.com/GovReady/govready/master/templates/issue-rhel6 > /etc/issue
```


# Run aquaduct

Install Git
```
sudo yum install git -y
```

Clone Aqueduct files (to run fix scripts)
```
git clone git://git.fedorahosted.org/git/aqueduct.git
cd aqueduct/compliance/bash/stig/rhel-6/dev/
sh Aqueduct-STIG.sh
cd /home/myfisma
```

# Rescan
Scan again

```
govready Scan

govready compare
```

# Package into vagrant base box
Clean up extraneous files
change ownership of /home/myfisma to vagrant
```
chown -R vagrant:vagrant /home/myfisma
```

Exit vagrant ssh
Halt vagrant instance, restart, make sure login works
```
vagrant halt centos65
vagrant up centos65
vagrant ssh centos65
exit
```
Open virtualbox and get full name of virtual instance 
Note: see: https://github.com/mitchellh/vagrant/issues/3827#issuecomment-52034501
Package vagrant base box
```
vagrant package --base REALNAME_from_virtualbox --output govready-centos-6.5-x86_64-noX-0.2.1-server-0.8.0.box
```


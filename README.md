# Vagrantfiles for Cisco Nexus 9000v

Configuration Notes

http://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/nx-osv/configuration/guide/b_NX-OSv_9000/b_NX-OSv_chapter_01.pdf

###Setting up Vagrant Environment

- Download *nxosv-final.7.0.3.I5.1.box* or latest from Cisco.com (Account required).
- move to purpose named directory
  Example: /d/Dropbox/scripts/vagrant/nxosv

  within ./nxosv directory:
  
```  
vagrant box add --name n9kv1 nxosv-final.7.0.3.I5.1.box
vagrant box list
vagrant init n9kv1

```

The vagrant box add command adds a box named "n9kv1" based on the *nxosv-final.7.0.3.I5.1.box* box image to your box repository.
The vagrant init command generates a single node, uncustomized VM instance.  This can be useful in certain situations and so I always save it as Vagrantfile.original.

```
Claudia@Mac-mini:~/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv$ vagrant box list
cisco-csr1kv-csr1000v-universalk9.03.16.03.S.155-3.S3 (virtualbox, 0)
n9kv1                                                 (virtualbox, 0)
ubuntu/trusty64                                       (virtualbox, 20160908.0.0)
vEOS-lab-4.16.6M                                      (virtualbox, 0)
Claudia@Mac-mini:~/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv$ 
```

## Vagrantfiles


Note: Started with Vagrantfile example from Configuration Notes

Out of the box, the Vagrantfile provided in the examples (Vagrantfile.mac) will error out on a Windows system on the "vagrant up" command because the box itself has the named pipe hardcoded as /tmp/test which is using the linux convention.  On a Mac that part does not error out.

Also out of the box on either Mac or Windows, you can't ssh into the VM and so this will need some work.  Basically its the same provisioning problem as that found on the csr1kv.

The Vagrantfile.win10 addresses the named pipe issue and you can actually use Putty to serial to the named pipe on the first node, break out of POAP, and but on a base configuration.  The second node does not come up so that will take some troubleshooting and neither can be sshed into.

Original Sammple Vagrantfile on Windows 10 Pro system:

```
claud@DESKTOP-S41OCM2 MINGW64 /d/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv
$ vagrant up
Bringing machine 'n9kv1' up with 'virtualbox' provider...
Bringing machine 'n9kv2' up with 'virtualbox' provider...
==> n9kv1: Clearing any previously set forwarded ports...
==> n9kv1: Clearing any previously set network interfaces...
==> n9kv1: Specific bridge 'en0: Ethernet' not found. You may be asked to specify
==> n9kv1: which network to bridge to.
==> n9kv1: Preparing network interfaces based on configuration...
    n9kv1: Adapter 1: nat
    n9kv1: Adapter 2: bridged
    n9kv1: Adapter 3: intnet
    n9kv1: Adapter 4: intnet
    n9kv1: Adapter 5: intnet
    n9kv1: Adapter 6: intnet
    n9kv1: Adapter 7: intnet
    n9kv1: Adapter 8: intnet
    n9kv1: Adapter 9: intnet
==> n9kv1: Forwarding ports...
    n9kv1: 80 (guest) => 8080 (host) (adapter 1)
    n9kv1: 22 (guest) => 2222 (host) (adapter 1)
==> n9kv1: Running 'pre-boot' VM customizations...
==> n9kv1: Booting VM...
There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["startvm", "ed4fadf7-4a55-4760-b613-a8908589ca57", "--type", "headless"]

Stderr: VBoxManage.EXE: error: NamedPipe#0 failed to create named pipe /tmp/test (VERR_INVALID_NAME)
VBoxManage.EXE: error: Details: code E_FAIL (0x80004005), component ConsoleWrap, interface IConsole

claud@DESKTOP-S41OCM2 MINGW64 /d/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv
```



Master Repository
claud@DESKTOP-S41OCM2 MINGW64 /d/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv
$ pwd
/d/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv

```
claud@DESKTOP-S41OCM2 MINGW64 /d/Dropbox (Indigo Wire Networks)/scripts/vagrant/nxosv
$ ls -al
total 759904
drwxr-xr-x 1 claud 197609         0 Nov 29 08:05 ./
drwxr-xr-x 1 claud 197609         0 Nov 28 17:05 ../
drwxr-xr-x 1 claud 197609         0 Nov 29 05:49 .vagrant/
-rw-r--r-- 1 claud 197609 778096640 Nov 28 12:22 nxosv-final.7.0.3.I5.1.box
-rw-r--r-- 1 claud 197609       881 Nov 29 08:05 README.md
-rw-r--r-- 1 claud 197609      4221 Nov 29 08:01 Vagrantfile
-rw-r--r-- 1 claud 197609      3356 Nov 29 06:02 Vagrantfile.2node
-rw-r--r-- 1 claud 197609      4221 Nov 29 07:55 Vagrantfile.mac
-rw-r--r-- 1 claud 197609      3080 Nov 29 05:39 Vagrantfile.original
-rw-r--r-- 1 claud 197609      4213 Nov 29 07:47 Vagrantfile.win10

```





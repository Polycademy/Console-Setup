Vagrant Setup
=============

Just download the Windows Vagrant. Make sure vagrant is executable on command line.

To fully destroy VM:

```
cd directoryThatContainsVagrantfile
vagrant destroy -f
VBoxManage unregistervm <vm> --delete # <vm> will be virtualbox vm name
vagrant box remove <box> # <box> is the box name, see box names on vagrant box list
```

For some reason, destroying simply destroys the instance that is loaded on the provider. The unextracted vagrant box still exists on your system. To delete that you need to do `vagrant box remove <box>`. Sometimes the vagrant destroy doesn't fully remove the instance from Virtualbox, that's when you need to unregistervm the VM. Boxes are stored in in `~/.vagrant.d/boxes`. VMs are stored in `~/VirtualBox VMs`. In the VM directory, it's not always obvious how VMs are stored. Basically there's a folder for the VM slot in Virtualbox, but there may also be an extra folder for the actual disk image. In fact 2 folders in the VM folder may use a third folder that holds the actual disk images. Why do we have these multiple folders? Well the idea being is you may have multiple projects using the vagrant box. Now that same vagrant box will extract into 2 different disk images. These 2 disk images may be placed in one folder. But then you have VM instances on Virtualbox, which requires 2 separate folders, and each will then reference their particular disk image that is inside the same folder. What we can see is that we have 3 things:

1. Boxes
2. Disks
3. VM Instances

Boxes are handled by Vagrant. Extracted they give out disks. Disks are the actual thing that's used by virtualisation providers. The VM providers need some place to put down metadata about the VM instance, and so they put them in the VM Instances folder. A VM Instance folder with a disk is an active VM instance!

Vagrant Keys
------------

Vagrant comes with default public keys. We may have to use them for other applications.

```
mkdir ~/.ssh/vagrant
curl https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant > ~/.ssh/vagrant/vagrant
curl https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub > ~/.ssh/vagrant/vagrant.pub
puttygen ~/.ssh/vagrant/vagrant -o ~/.ssh/vagrant.ppk
```
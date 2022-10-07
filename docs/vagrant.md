# Vagrant


## concept

[Source](https://documentation.suse.com/sles/15-SP2/html/SLES-all/cha-libvirt-manage-vagrant.html#sec-libvirt-vagrant-intro)

Vagrant uses providers, provisioners, boxes, and Vagrantfiles as building blocks of the virtual machines.

- Provider: Services to set up and create virtual environments (e.g. VirtualBox)
- Provisioner: Tools to customize the configuration of virtual environments. Vagrant has built built-in providers for uploading files, syncing directories or executing shell commands
- Vagrantfile: Configuration file and file name (Vagrantfile) for virtual environments.
- Box: Format and an extension (*.box) for virtual environments. Boxes can be downloaded from the Vagrant Cloud and copied from one machine to another in order to to replicate an environment.

## Vagrant file + Boxes

Initially created with cli and a box name e.g: `vagrant init opensuse/Tumbleweed.x86_64`.

You can find public available boxes on [app.vagrantup.com](https://app.vagrantup.com/boxes/search).

## autocompletion

```shell
vagrant autocomplete install --bash ##--zsh
vagrant box list
```

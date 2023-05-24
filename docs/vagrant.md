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

## reaload provosion

```shell
vagrant reload --provision 
# or 
vagrant destroy -f && vagrant up
```

## SSH into vagrant box without 'vagrant ssh'

```shell
# save the config to a file
vagrant ssh-config > vagrant-ssh

# run ssh with the file.
ssh -F vagrant-ssh default
```

[Source](https://stackoverflow.com/questions/10864372/how-to-ssh-to-vagrant-without-actually-running-vagrant-ssh)

## connect with ansible to vagrant box

```shell
vagrant ssh-config > vagrant-ssh.cfg
```

You can refence the ssh config in ansible by adding the following to a host_var file:

```yaml
ansible_ssh_common_args: '-F ./host_vars/VAGRANT/vagrant-ssh.cfg'
# must match host in vagrant-ssh.cfg
ansible_ssh_host: default
```

## autocompletion

```shell
vagrant autocomplete install --bash ##--zsh
vagrant box list
```

## increase ram and cpu 

Add the following to Vagrantfile 

```shell
  # 4 gb, 2 cores
  config.vm.provider "virtualbox" do |v|
    v.memory = 4000
    v.cpus = 2
  end
```

## increase swap size

Is controled by vm image. If you want to increase the size you ned to run a  provision script by adding the following to Vagrantfile: 

```shell
  config.vm.provision "shell", path: "./increase_swap_space.sh"
```

The content of ./increase_swap_space.sh could look like this ([Source](https://programmaticponderings.com/2013/12/19/scripting-linux-swap-space/)):  

```shell
#!/bin/sh

# size of swapfile in megabytes
swapsize=512

# does the swap file already exist?
grep -q "swapfile" /etc/fstab

# if not then create it
if [ $? -ne 0 ]; then
    echo 'swapfile not found. Adding swapfile.'
    fallocate -l ${swapsize}M /swapfile
    chmod 600 /swapfile
    mkswap /swapfile
    swapon /swapfile
    echo '/swapfile none swap defaults 0 0' >> /etc/fstab
else
    echo 'swapfile found. No changes made.'
fi

# output results to terminal
cat /proc/swaps
cat /proc/meminfo | grep Swap
```

## forward port

```shell
config.vm.network "forwarded_port", guest: 8080, host: 8080
```

## Copy files

```shell
config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"
```


# linux operations (without containers)

## misc

- find file anywhere: `sudo locate filename` (index is updated once a day. recreate with `sudo updatedb`)
- find a file which contains a string : `grep -nr 'search*' /opt/app/`
- find where a on the path is saved: `which execuableName`

## check hardware

- `lshw` - cpu cores and frequency and ram
- `lsblk` - check disk size

## permissions

![image-linux-file-permission](../img/fig_permissions.jpg) 

[Source of image](http://www.csit.parkland.edu/~smauney/csc128/permissions_and_links.html)

- [Unix permission bit calculator](http://permissions-calculator.org/)
- not binary format
  - Roles:
      - u = user
      - g = group
      - o = other
  - Permission:
      - r = read
      - w = write
      - x = execute
- You can also use binary format
  - three numbers (e.g. 755)
    - first: user
    - second: group
    - third: other
  - value of permissions
    - read: 4
    - write: 2
    - execute: 1
- default permission bits are defined by the umask

You can set the owner user and the owner group of a file with `chown`.

```shell
# set user and group as ownder of the direcoty and all its content
sudo chown -R user:group /var/myfolder
```

To handle the rights of a file you can use `chmod`. 

```shell
# add execute rights for the (owner) user
sudo chmod u+x /opt/myapp/config/start.sh
# remove execute rights for others 
sudo chmod o-x /opt/myapp/config/start.sh

# remove write and read rights from the group of a file 
sudo chmod g-rw /opt/myapp/config/config.properties
# remove  write and read rights from the group of a file for every file in the directory 
sudo chmod -R g-rw /opt/myapp/config/

```

## root

- `sudo`
- `su`
- `su -` changes environment (e.g ~/bashrc, )

## install application

Assumes that you need to use tar balls. If you use a package manager most or all of the steps will be done automatically depending on the package manager you used. 

1. add a user and a group
2. download tar and extract it to /opt/
3. configure application to save data and log outside of the installation folder
4. define a service for the application


## user management

### Add user 

```shell
# list users
cat /etc/passwd

# create user
sudo useradd username
```

### Add user to group (e.g. to sudo group): 

```shell
# list groups 
cat /etc/group

# list groups for user
id username

# create group
sudo groupadd groupname

# add user to group
sudo usermod -aG groupname username
# For the changes to take effect you need to logout the user.OR it may suffice to run one of the following commands:
# newgrp - groupname
# sudo su - $USER
# when using vscode remote kill the remote vs code server

# add to sudo group (sudoers) 
sudo usermod -aG sudo newuser
# you may want to take a look into the file /etc/sudoers if the group sudo does not exist
```

### Add public ssh key to user 

e.g. for connecting remotely to this user with the corresponding private key

```shell
# create a ~/.ssh/ folder if it does not exist
mkdir -p ~/.ssh/
printf "<content of pub key>" >>  ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# ALTERNATIVE: Execute from remote to add your own key to the server (works only if you have a password or other authentification mechanism yet)
ssh-copy-id -i ~/.ssh/mykey user@host
```

### Add private ssh key to user 

e.g. authentication on another machine where the public/private key pair is authorized

```Shell
# either in /etc/sudoers.d/local

# or just add to a special group which might be sudo
sudo adduser $USER sudo
# or another special group like operator or such

# to disable password prompt when using sudo: 
## opens /etc/sudoers but with syntax check
sudo visudo  
```

### Allow user to use sudo

```Shell

TODO
```

### tar - unzip files

You have the following tar file: softwarename-1.2.3.tar.gz

- Unzip with wildcard (e.g. to omit version): `tar xzvf softwarename-*.tar.gz` -> folder: softwarename-1.2.3
- Unzip to a target folder: `tar xzvf softwarename-1.2.3.tar.gz -C /opt` -> folder: /opt/softwarename-1.2.3
- Unzip to a target folder and set a new folder name: `tar xzvf softwarename-1.2.3.tar.gz && mv softwarename-1.2.3/* /opt/softwarename` -> folder: /opt/softwarename


### systemd - run an application as a service

A service is a way to start a application as a daemon process. How to create a service depends on which linux you use. The old system is called system V ("system five") (initd). The following  assumes that you use the newer systemd.

#### why?

- [systemd](https://fedoramagazine.org/systemd-getting-a-grip-on-units/)
- [systemd unit files](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files)

#### how

create a service in systemd you need to create a so called unit file:

- in direcotry /etc/systemd/system/
- File with ending '.service'
- containes '[Unit]' and '[Service]' and '[Install]' block


Interact with a service:

- Status of service: `systemctl start`
- Start service: `systemctl start`
- Stop service: `systemctl stop`
- Restart service: `systemctl restart`
- At system startup: `systemctl enable`
- At system startup: `systemctl disable`

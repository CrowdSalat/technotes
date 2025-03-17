# red hat cockpit

1. cockpit server to connect via browser
2. cockpit client which runs on you workstation and does not need a cockpit server


## use cockpit workstation client

1. type "localhost" to connect to local machine
2. type `user@ip` or `user@dns` to connect to remote. Allows to use ssh key as well as password.

## use cockpit server

Runs on port 9090.

Can be used on host it runs on. The [Host switcher](https://cockpit-project.org/blog/cockpit-322.html) feature is deprecated and tzhe connection feature only allows username + password and not ssh. 

## install 

### cockpit server 

```shell
sudo dnf install cockpit -y
sudo systemctl enable --now cockpit.socket
sudo dnf install cockpit-podman -y
sudo dnf install cockpit-machines -y
sudo dnf install cockpit-files -y
```

*Only allows to connect to other servers via password. Does not support to connect to other server with ssh.*

### cockpit workstation client

[FlatHub page](https://flathub.org/apps/org.cockpit_project.CockpitClient)
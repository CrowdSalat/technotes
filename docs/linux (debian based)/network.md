# network stuff

## ssh

```shell
# add -v for verbose mode

# connect
ssh <user>@<ip or url>

# connect on non standard port
ssh -p 12345 <user>@<ip or url>

# connect use rsa private key saved under ~/.ssh/ instead of password
ssh -i ~/.ssh/id_rsa <user>@<ip or url>

# ssh tunnel which connects my local port 1025 with the remote port 4000 over the standard ssh port 22
ssh -L1025:192.168.x.x:4000 -v <user>@<ip or url>
# can be used via
ssh -p1025 <user>@192.168.x.x

```

[nice explanation of ssh tunnels](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)

## ssh keys

The -i flag can be omitted if the standard file names id_rsa (private key) and id_rsa.pub (public key) are used.

```shell
#generate a private and public key pair in ~/.ssh/ folder
ssh-keygen

# your user: copy your public key to the authorized_keys file in your home directory on a remote machine
# allows access with your private key instead of a password
ssh-copy-id -i ~/.ssh/id_rsa user@host

# other user:  copy your public key to the authorized_keys file in another home direcotry
cat ~/id_rsa.pub | ssh your_user@remote.server.com “sudo tee -a /USER/.ssh/authorized_keys”

# connect use rsa private key saved under ~/.ssh/ instead of password
ssh -i ~/.ssh/id_rsa <user>@<ip or url>

# Known hosts are saved in  following locatoins
## as entry in /etc/ssh/ssh_known_hosts
## as entry in /etc/ssh/ssh_config
## as *.pub file in /etc/ssh/ folder


# permisions of pub files
chmod 700 ~/.ssh
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/id_rsa
```

## known_hosts

```shell
# remove old entry from known_host
ssh-keygen -R SERVER_NAME -f ~/.ssh/known_hosts
# add new key to known_hosts
ssh-keyscan SERVER_NAME >> ~/.ssh/known_hosts
```

## check open ports

```shell
# local machine: check for open ports and which process is listening on them
sudo netstat -lptu 

# local machine: check which process listens on port
lsof -i :8080

# remote machine: check for open tcp ports (scans the first 1000 ports)
nmap <ip_address>

# remote machine: check for open tcp ports (scans all ports)
nmap -p- <ip_address>

# remote machine: check if tcp port 8080 is open 
nmap -p 8080 <ip_address>

# remote machine: start webserver on port and check from browser on local machine
mkdir tmp && cd tmp && echo "Hello, you reached Jans webserver" > index.html
python3 -m http.server 8080 
cd -
```

with windows powershell:

```
tnc google.com -port 80
```

## open ports (iptables)

```shell
# lists all rules
sudo iptables -L 

# save all rules
iptables-save > ipconfig_rules_backup.txt 

# restore old rules
iptables-restore < ipconfig_rules_backup.txt

# add new rule syntax
# add new rule example
TODO
```

## generate a certificate

```shell
openssl req -x509 -newkey rsa:4096 -keyout ./key.pem -out ./cert.pem -days 365
```

## check firewall 

```shell
#!/bin/bash
 
servers=(
„beispiel“
)
 
echo "Script startetd from: $(hostname -f)"
 
for server in "${servers[@]}"
do
  target_url=https://echo.example.de
  echo -n "Try to call $target_url from $server. "
  command="curl --max-time 5 -w 'HTTP Code %{http_code}\n' -o /dev/null --silent $target_url"
  ssh "$server" "$command"
done
```

## remove old ssh key of host for multiple users

```shell
#!/bin/bash

echo "Current user: $(whoami)"

HOST="myhost.example.de"

USERS=(
"user1"
"user2"
)
echo "Reset $HOST SSH Hostkey"

for USER in "${USERS[@]}"
do
    echo "Reset SSH Hostkey for user $USER"
    HOME_DIR="/home1/users/$USER"

    echo "Remove $HOST from ${HOME_DIR}/.ssh/known_hosts"
    sudo ssh-keygen -R "$HOST" -f ${HOME_DIR}/.ssh/known_hosts
   
    echo "Add $HOST to ${HOME_DIR}/.ssh/known_hosts"
    sudo ssh-keyscan "$HOST" | sudo tee --append ${HOME_DIR}/.ssh/known_hosts

done
```

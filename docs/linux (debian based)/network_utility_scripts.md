# Network shell snipptes

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

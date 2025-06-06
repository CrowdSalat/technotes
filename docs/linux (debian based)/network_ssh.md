# SSH


## generate ssh key

The -i flag for connection can be omitted if the standard file names id_ALGORITHM (private key) and id_ALGORITHM.pub (public key) are used.

```shell
# generate a private and public key pair in ~/.ssh/ folder
ssh-keygen -t ed25519 -b 4096

# in specifig folder
ssh-keygen -t ed25519 -b 4096 -f /tmp/

# needed permisions of files. Normally created automatically
chmod 700 ~/.ssh
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/id_ed25519
```

If the "-----BEGIN OPENSSH PRIVATE KEY-----" format is not accepted use plain openssl cli:

```shell
openssl genpkey -algorithm ed25519 -out key.pem
openssl pkey -in key.pem -pubout -out public.pem

# or for rsa
openssl genrsa -out key.pem 4096
openssl rsa -in key.pem -out pubkey.pem -outform PEM -pubout
```

## connect via ssh

```shell
# add -v for verbose mode

# connect
ssh <user>@<ip or url>

# connect on non standard port
ssh -p 12345 <user>@<ip or url>

# connect use rsa private key saved under ~/.ssh/ instead of password
ssh -i ~/.ssh/id_rsa <user>@<ip or url>
```

## ssh tunnel

[nice explanation of ssh tunnels](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)

```shell
# ssh tunnel which connects my local port 1025 with the remote port 4000 over the standard ssh port 22
ssh -L1025:192.168.x.x:4000 -v <user>@<ip or url>

# if ssh is forwarded it can be used via -p flag
ssh -p1025 <user>@192.168.x.x
```

## handle identity with password

For .bashrc or .zshrc to cache password on startup if not cached already:

```shell
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval "$(ssh-agent -s)" > /dev/null
fi

# Function to check if the key is already added
check_key_added() {
  ssh-add -l | grep -q "$(ssh-keygen -lf ~/.ssh/id_ed25519 | awk '{print $2}')"
}

# Add the key if it's not already added
if ! check_key_added; then
  echo "Add ssh key for 8 hours"
  ssh-add -t 28800 ~/.ssh/id_ed25519 # 8 hours timeout
fi
```

## trust ssh key on remote server

```shell
# your user: copy your public key to the authorized_keys file in your home directory on a remote machine
# allows access with your private key instead of a password
ssh-copy-id -i ~/.ssh/id_rsa user@host

# other user:  copy your public key to the authorized_keys file in another home direcotry
cat ~/id_rsa.pub | ssh your_user@remote.server.com “sudo tee -a /USER/.ssh/authorized_keys”
```

## location of known_hosts

Known hosts are saved in  following locations
- as entry in /etc/ssh/ssh_known_hosts
- as entry in /etc/ssh/ssh_config
- as *.pub file in /etc/ssh/ folder


## add and remove entries from known_hosts 

```shell
# remove old entry from known_host
ssh-keygen -R SERVER_NAME -f ~/.ssh/known_hosts
# add new key to known_hosts
ssh-keyscan SERVER_NAME >> ~/.ssh/known_hosts

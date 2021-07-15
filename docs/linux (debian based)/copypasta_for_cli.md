# Copy&Pasta CLI installations & config

## homebrew - add to path

```
echo 'export PATH=$PATH:/home/linuxbrew/.linuxbrew/bin/' >> ~/.bashrc && source ~/.bashrc && brew help
```

## docker - on debian

[You cannot use linuxbrew to install docker engine](https://github.com/Linuxbrew/brew/issues/723). SO you need to install it manually. Brew can however be used to install docker cli with bashcompletion.

```shell
# install docker 
## you may want to check the script before running it
## source: https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# allow current user to manage docker
## source: https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script
sudo groupadd docker
sudo usermod -aG docker $USER
## log out current user
docker run hello-world
```

## bash completion

### any binary installed with brew

If the binary is installed with brew there is a chance that the bash completion script is genereted to the `$(brew --prefix)/etc/bash_completion.d/` direcoty. If so brew will tell you at the end of the installation. 

```shell
# run once after installing brew
echo 'for BREW_COMPLETION in "$(brew --prefix)/etc/bash_completion.d/"*; do 
    [[ -r "$BREW_COMPLETION" ]] && source "$BREW_COMPLETION" 
done' \
>>~/.bashrc && source ~/.bashrc

# after brew added a new completion script just source 
source ~/.bashrc
```

### kubectl - bash auto-completion

```shell
echo 'source <(kubectl completion bash)' >>~/.bashrc && source ~/.bashrc
```

### helm - bash auto-completion

```shell
echo 'source <(helm completion bash)' >>~/.bashrc && source ~/.bashrc
```

### k3d 

bash auto-completion

```shell
echo 'source <(k3d completion bash)' >>~/.bashrc && source ~/.bashrc
```

Create cluster

```shell
k3d cluster create
```

### git

```shell
# alias
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# ssh keygen
ssh-keygen -t rsa -b 4096

# remember ssh password
ssh-add ~/.ssh/*_rsa


```

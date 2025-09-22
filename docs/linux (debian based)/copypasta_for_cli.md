# Copy&Pasta CLI installations & config

## create multiple direcotries with one command

```shell
mkdir -p applications/my-new-app/{base,overlays/{dev,staging,prod}}
```

The shell expands this into:

- applications/my-new-app/base
- applications/my-new-app/overlays/dev
- applications/my-new-app/overlays/staging
- applications/my-new-app/overlays/prod

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
## k8s ecosystem

```
# kubectl
brew install kubernetes-cli
kubectl version

# kubectl package manager
brew install krew
/home/linuxbrew/.linuxbrew/bin/kubectl-krew install krew
echo 'export PATH=${PATH}:~/.krew/bin/' >> ~/.bashrc && source ~/.bashrc

## for changing context Usage: kubectl ctx
kubectl krew install ctx

## for changing namespace Usage: kubectl ns
kubectl krew install ns

## for merging k8s config files. Usage: kubectl konfig
kubectl krew install konfig
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

Add image from local container host to k3d cluster (# prerequisite see [k3d page](../k3d.md))

```shell
k3d image import IMAGENAME --cluster local-cluster
```

### git

```shell
# alias
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# ssh keygen
ssh-keygen -t ed25519 

# remember ssh password
ssh-add ~/.ssh/*_rsa


```

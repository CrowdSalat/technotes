# Notes on mac usage

## moving from linux to mac

- [moving to zsh](https://scriptingosx.com/2019/06/moving-to-zsh/)
- [macos - how does the window system work](https://apple.stackexchange.com/questions/339214/whats-the-difference-between-minimize-and-hide-between-maximize-and-fullscreen)

## install snippets

```shell

## installm brew: https://brew.sh/
## install oh-my-zsh: https://ohmyz.sh/#install

# add brew to path
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# activate completion in zsh
echo "autoload -Uz compinit && compinit" >> ~/.zshrc
. ~/.zshrc

brew install --cask firefox
brew install --cask visual-studio-code
# control dark mode
brew install --cask nightfall

# cli
brew install jq
brew install tfenv
# nvm needs additional steps see brew low after installs
brew install thefuck
brew install yarn
brew install nvm

# python
brew install pyenv
pyenv install -l
pyenv install 3.8.0
pyenv install 2.7.14

pyenv local 2.8.0

# docker & Kubernetes
## Option 1: Rancher Desktop
brew install --cask rancher
echo "# rancher desktop" >> ~/.zshrc
echo "export PATH=$PATH:$HOME/.rd/bin" >> ~/.zshrc
source ~/.zshrc

## Option 2: colima provides container runtime in vm
brew install colima
##  docker cli
brew install docker

## docker mac fix
# problems wth keychain when running docker login:
# to fix: brew install docker-credential-helper

# cloud
brew install awscli
brew install --cask google-cloud-sdk
brew install azure-cli



```
## ~/.zprofile

```shell
## brew completion when zsh is installed
## source: https://docs.brew.sh/Shell-Completion
FPATH="$(brew --prefix)/share/zsh/site-functions:${FPATH}"
```

## ~/.zshrc snippets

```shell

# oh my zsh stuff... 

# activate completion in zsh
autoload -Uz compinit && compinit

# add brew to path
eval "$(/opt/homebrew/bin/brew shellenv)"

# for git
ssh-add --apple-use-keychain ~/.ssh/id_rsa

# brew completion
if type brew &>/dev/null
then
  FPATH="$(brew --prefix)/share/zsh/site-functions:${FPATH}"

  autoload -Uz compinit
  compinit
fi

# from brew install info
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

# gpg
export GPG_TTY=$(tty)

eval $(thefuck --alias)

```

## usual git stuff

```shell
## alias
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

## base config
MAIL_ADDRESS="jan.weyrich@viadee.de"
git config --global user.name "Jan Weyrich"
git config --global user.email "$MAIL_ADDRESS"

git config --global core.hooksPath .githooks

## generate ssh key
ssh-keygen -t rsa -b 4096 -C "$MAIL_ADDRESS"

# add ssh key agent for passwort prompt
# see https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent

## gpg
brew install gpg2 gnupg pinentry-mac

gpg --full-generate-key

# config for git
gpg -K --keyid-format SHORT
export GPG_SEC_ID=8627004B
gpg --armor --export $GPG_SEC_ID > ~/gpg-public.pem
git config --global user.signingkey $GPG_SEC_ID
git config --global gpg.program $(which gpg)
git config --global commit.gpgsign true
echo 'export GPG_TTY=$(tty)' >> ~/.zshrc
source ~/.zshrc

# for pw prompt see here: https://unixb0y.de/blog/articles/2019-01/gpg-password-macos-keychain
echo 'use-agent' > ~/.gnupg/gpg.conf
echo "pinentry-program /opt/homebrew/bin/pinentry-mac" > ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent

# check with
echo "test" | gpg --clearsign

# if you got: gpg: signing failed: No pinentry
# you may want to try with:
echo "/opt/homebrew/bin/pinentry" > ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```

## colima 

```shell
# to debug
export DEBUG=testcontainers

colima start
docker context use colima
docker run hello-world
```

Could not get testcontainers to work with colima. Here are some links with further information:

- https://github.com/testcontainers/testcontainers-java/issues/5034#issuecomment-1036433226
- https://stackoverflow.com/questions/70749679/how-can-i-use-testcontainer-in-mac-os-with-out-docker-desktop

## m1 compatibility problems

## docker build

```
echo "# for docker build on m1" >> ~/.zshrc
echo "export DOCKER_BUILDKIT=0" >> ~/.zshrc
echo "export COMPOSE_DOCKER_CLI_BUILD=0" >> ~/.zshrc

source ~/.zshrc
```

```shell
failed to solve with frontend dockerfile.v0: failed to create LLB definition: rpc error: code = Unknown desc = error getting credentials - err: docker-credential-osxkeychain resolves to executable in current directory (./docker-credential-osxkeychain), out: ``
```

[Source](https://stackoverflow.com/a/66695181)

### terraform

`Provider registry.terraform.io/hashicorp/template v2.2.0 does not have a package available for your current platform, darwin_arm64.`

[Terraform forum](https://discuss.hashicorp.com/t/template-v2-2-0-does-not-have-a-package-available-mac-m1/35099/7)

```shell
brew install kreuzwerker/taps/m1-terraform-provider-helper
m1-terraform-provider-helper activate
m1-terraform-provider-helper install hashicorp/template -v v2.2.0
```

## extend sudoers file

´´´shell
sudo visudo /private/etc/sudoers

# add

# jwy ALL=(ALL) ALL
´´´

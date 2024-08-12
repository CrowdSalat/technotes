# oh-my-zsh

## used plugins

see ~/.zshrc

```shell
# ZSH_THEME="robbyrussell"
ZSH_THEME="powerlevel10k"
...
plugins=(git podman zsh-syntax-highlighting you-should-use)
```

## prerequisite

```shell
# zsh
sudo dnf install zsh
chsh -s $(which zsh)

# oh-my-zsh
##TODO or link to webpage

# oh-my-zsh theme
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
# font is missing and can be installed with help of
## [GitHub - romkatv/powerlevel10k: A Zsh theme](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#meslo-nerd-font-patched-for-powerlevel10k)

# # oh-my-zsh plugin install 
# list of automatically installed plugins https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/docker/README.md 
# zsh-syntax-highlight
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

## [You-should-use Plugin](https://github.com/MichaelAquilina/zsh-you-should-use?ref=catalins.tech)
git clone https://github.com/MichaelAquilina/zsh-you-should-use.git $ZSH_CUSTOM/plugins/you-should-use
sudo dnf install python-pygments
# usage ccat or cless to have highlighting
```


## powerlevel10k

Changes quite a bit of behavior. 

e.g. "Transient prompt": removes the path after execution so it is easier to copy. Change it with `p10k configure`. 

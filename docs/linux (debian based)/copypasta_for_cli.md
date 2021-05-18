# Copy&Pasta CLI installations & config

## homebrew - add to path

```
echo 'export PATH=$PATH:/home/linuxbrew/.linuxbrew/bin/' >> ~/.bashrc && source ~/.bashrc && brew help
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

```
echo 'source <(kubectl completion bash)' >>~/.bashrc && source ~/.bashrc
```

### helm - bash auto-completion

```
echo 'source <(helm completion bash)' >>~/.bashrc && source ~/.bashrc
```

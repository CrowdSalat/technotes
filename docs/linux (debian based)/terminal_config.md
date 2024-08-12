# Terminal/Shell config

## alias

```shell
# Linux version of OSX pbcopy and pbpaste.
alias pbcopy=’xsel — clipboard — input’
alias pbpaste=’xsel — clipboard — output’

# copy last command to clipboard
alias copyLastCmd='fc -ln -1 | awk '\''{$1=$1}1'\'' ORS='\'''\'' | pbcopy'
```

## Terminal config

### iterm2

## Shell config

### Bash

### Zsh

### Fsh

## copy from terminal


### mac

Use pbcopy and pbpaste.

```shell
# Copy Hello\n to clipboard
echo "Hello" | pbcopy

# copy last command to clipboard
alias copyLastCmd='fc -ln -1 | awk '\''{$1=$1}1'\'' ORS='\'''\'' | pbcopy'
copyLastCmd
```

### linux

Use pbcopy and pbpaste.

```shell
# Linux version of OSX pbcopy and pbpaste.
alias pbcopy=’xsel — clipboard — input’
alias pbpaste=’xsel — clipboard — output’

# Copy Hello\n to clipboard
echo "Hello" | pbcopy

# copy last command to clipboard
alias copyLastCmd='fc -ln -1 | awk '\''{$1=$1}1'\'' ORS='\'''\'' | pbcopy'
copyLastCmd
```

## Sources

- [StackExchange](https://apple.stackexchange.com/questions/110343/copy-last-command-in-terminal)

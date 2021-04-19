# Copy&Pasta CLI installations & config

## homebrew - add to path

```
echo 'export PATH=$PATH:/home/linuxbrew/.linuxbrew/bin/' >> ~/.bashrc && source ~/.bashrc && brew help
```

## kubectl - bash auto-completion

```
echo 'source <(kubectl completion bash)' >>~/.bashrc && source ~/.bashrc
```

## helm - bash auto-completion

```
echo 'source <(helm completion bash)' >>~/.bashrc && source ~/.bashrc
```
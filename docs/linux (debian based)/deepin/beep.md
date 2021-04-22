# Disable hardware beep 

## completely

```shell
# temporarly until restart
sudo rmmod pcspkr

# to disable loading of moduke at startup
echo "blacklist pcspkr" >> /etc/modprobe.d/blacklist
```

## firefox only

-`Open `about:config`
- Set `accessibility.typeaheadfind.enablesound` to false

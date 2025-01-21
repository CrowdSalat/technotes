# Citrix troubleshoot

## keyboard layout

The linux client might use the false keyboard layout if the system language contradicts with the layout. To change this behavio edit the file ~/.ICAClient/All_Regions.ini. After that you might need to restart the Citrix Client.

```shell
# open folder
code ~/.ICAClient/All_Regions.ini

# set
# KeyboardLayout=German
```

## maximize window issue

Maximizing windows on linux might lead to a white bar on the lower side of the monitor or to duplication of the image. It also seems to be connected to a loose of connection between keyboard and citrix.

Citrix does not support wayland ([12.11.2024](https://docs.citrix.com/en-us/citrix-workspace-app-for-linux/system-requirements.html)), but only X11 or X.Org. There is a [preview](https://docs.citrix.com/en-us/linux-virtual-delivery-agent/current-release/configure/wayland.html) which supports wayland. The solution is not tested because I used a internal containerized version of citrix which just worked on linux.


## Map Mac keyboard layout to windows

Working solution ss described in your obsidian notes.

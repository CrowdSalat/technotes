# Citrix troubleshoot

## keyboard layout

The linux client might use the false keyboard layout if the system language contradicts with the layout. To change this behavio edit the file ~/.ICAClient/All_Regions.ini. After that you might need to restart the Citrix Client.

```shell
# open folder
code ~/.ICAClient/All_Regions.ini

# set
# KeyboardLayout=German
```

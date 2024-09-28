# Proxy

## Windows

In order to be used by cmd as well as the browsers you need to set the environmental variables:

HTTP_PROXY
HTTPS_PROXY
NO_PROXY

Set it via the environmental variables settings ui or via cmd with setx.

```shell
# show current proxies 
netsh winhttp show proxy

# set env vars
setx HTTP_PROXY myproxy.de
setx HTTPS_PROXY myproxy.de
setx NO_PROXY ".mycompanydomain.de,localhost,127.0.0.*"
```

## Linux

```shell
export HTTP_PROXY=myproxy.de
export HTTPS_PROXY=myproxy.de
export NO_PROXY=".mycompanydomain.de,localhost,127.0.0.*"
```

# Proxy with authentification

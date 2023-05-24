# Apache webserver

[GitHub Repo to test Proxy conf](https://github.com/CrowdSalat/nginx-proxypass-test)

## how to

### configure

Directories:

- Config `/etc/nginx/nginx.conf` set global settings and loads the config files in following two folders.
- All files in `/etc/nginx/conf.d/` which ends with .conf are loaded (only servers block allowed. No event or http blocl because it is already present in `/etc/nginx/nginx.conf`)
- (non container version) All files in `/etc/nginx/sites-enabled` which are usually only a symlink to files in `/etc/nginx/sites-available`. This directory contains the *default* configuration of nginx. The default settings exposes files in `/var/www` directory.

Commands:

- `nginx` - start server
- `nginx -s stop` - stop the server
- `nginx -s reload` - reload the config
- `nginx -t` - test config

Directives:

- A **server block** is a subset of Nginx’s configuration that defines a virtual server used to handle requests of a defined type
- A **location block** lives within a server block and is used to define how Nginx should handle requests for different resources and URIs for the parent server. A location can either be defined by a prefix string, or by a regular expression. Regular expressions are specified with the preceding “~*” modifier (for case-insensitive matching), or the “~” modifier (for case-sensitive matching). [see](https://nginx.org/en/docs/http/ngx_http_core_module.html#location)

- [Getting started](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/)
- [Reference core module](https://nginx.org/en/docs/http/ngx_http_core_module.html)
- [Reference proxy module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)

### see logs

Logs usually are saved in `/var/log/nginx`.

### configure reverse proxy for service

- [Reference proxy module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)
- [Simple examples](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/)

#### simple

```conf
server {
    listen       80;
    server_name  proxyhost.de;
    # call to proxyhost.de:80 get forwarded to 127.0.0.1:8008
    location / {
        # Sets the protocol and address of a proxied server and an optional URI. 
        ## https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass
        proxy_pass   http://127.0.0.1:8008;
    }
}
```

#### keep client information

- [Forwarded-XXXX Header explained](https://http.dev/forwarded) (used to be X-Forwarded)
- [List of variables build in nginx](http://nginx.org/en/docs/varindex.html)
- [How to work with paths](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/#passing-a-request-to-a-proxied-server)

```conf
server {
    listen       80;
    server_name  proxyhost.de;

    # call to proxyhost.de:80 get forwarded to 127.0.0.1:8008
    location / {
        # Sets the protocol and address of a proxied server and an optional URI. 
        ## https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass
        proxy_pass   http://backend.de:8008;
        proxy_set_header Forwarded-Host $host:$server_port;
        proxy_set_header Forwarded-Server $host;
        proxy_set_header Forwarded-For $proxy_add_x_forwarded_for;        }
}
```

#### path examples

Test your proxy_pass with this [docker-compose setup](https://github.com/CrowdSalat/nginx-proxypass-test)

##### proxy_pass without path (<protocoll>:<host>)

Location path will get appended:

```conf
...
        # proxyhost.de:80 -> 127.0.0.1:2000
        # proxyhost.de:80/example -> 127.0.0.1:2000/example
        location / {
            proxy_pass   http://127.0.0.1:2000;
        }

        # proxyhost.de:80/api -> 127.0.0.1:3000/api
        # proxyhost.de:80/api/v1 -> 127.0.0.1:3000/api/v1
        location /api {
            proxy_pass   http://127.0.0.1:3000;
        }
```

### proxy_pass with path (<protocoll>:<host>/<path>)

Location path will be replaced:

```conf
...
        # proxyhost.de/app -> 127.0.0.1:4000/api2
        # proxyhost.de/app/api2 -> 127.0.0.1:4000/api2/api2
        location /app {
            proxy_pass   http://127.0.0.1:4000/api2;
        }
        # => /app get replaced by /api2

        # proxyhost.de/app2 -> 127.0.0.1:5000/
        # proxyhost.de/app2/api/v1 -> 127.0.0.1:5000/api/v1
        location /app2 {
            # / at the end counts a path!
            proxy_pass   http://127.0.0.1:5000/;
        }
        # => /app2 get replaced by /
...
```

### add tls certificates

[Nginx TLS Guide](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/)

```conf
events { }
http {
    server {
        listen                      443 ssl;
        server_name                 www.example.com;
        ssl_certificate             /etc/nginx/conf/www.example.com.crt;
        ssl_certificate_key         /etc/nginx/conf/www.example.com.key;
        ssl_protocols               TLSv1.2 TLSv1.3;
        ssl_ciphers                 ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA;
        ssl_prefer_server_ciphers   off;

        location / {
            proxy_pass   http://127.0.0.1:8008;
        }
    }
}
```

### Connect to ldap

## Precedence of server block and location block

The [guide](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms) on how config blocks are chosen by nginx is helpful. The outline is:

1. find server block with the most 'specific' ip address and port in the **listen** directive 
2. if multiple listen directives are equally specific (or just equal) then the **server_name** directive will decide which will execute it. Search for the most 'specific one'

## example

Example of reverse proxy with tls support which proxies to a non-tls server (except for the subdomain it is generated the [tool](https://www.digitalocean.com/community/tools/nginx)):

```conf
server {
    listen 78.47.255.193:443 ssl http2;
    listen [::]:443 ssl http2;

    server_name weyrich.dev;
    root /var/www/weyrich.dev/public;

    # SSL
    ssl_certificate /etc/letsencrypt/live/weyrich.dev/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/weyrich.dev/privkey.pem;

    # security
    include nginxconfig.io/security.conf;

    # logging
    access_log /var/log/nginx/weyrich.dev.access.log;
    error_log /var/log/nginx/weyrich.dev.error.log warn;

    # index.html fallback
    location / {
        try_files $uri $uri/ /index.html;
    }

    # additional config
    include nginxconfig.io/general.conf;
}

# ci subdomains proxy pass
server {
    listen 78.47.255.193:443 ssl http2;
    listen [::]:443 ssl http2;

    server_name ci.weyrich.dev;

    # security
    include nginxconfig.io/security.conf;

    # logging
    access_log /var/log/nginx/ci.weyrich.dev.access.log;
    error_log /var/log/nginx/ci.weyrich.dev.error.log warn;

    # reverse proxy
    location / {
        proxy_pass http://127.0.0.1:180/;
        include nginxconfig.io/proxy.conf;
    }
}

# HTTP redirect
server {
    listen 78.47.255.193:80;
    listen [::]:80;

    server_name .weyrich.dev;

    return 301 https://weyrich.dev$request_uri;
}
```

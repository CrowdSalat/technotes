# Apache http webserver (httpd)

## how to

### modules for the given examples

Add at the following modules to use the following examples:

- headers
- proxy
- proxy_connect
- proxy_ajp
- proxy_http
- proxy_html
- proxy_wstunnel
- ssl

### configure reverse proxy for service

```shell
<VirtualHost *:80>
    ServerName backend.example.de
    ServerAlias backend.example.de

    ErrorLog /var/log/apache2/reverseproxy-error_log
    CustomLog /var/log/apache2/reverseproxy-access_log combined

    ProxyPass "/" "http://backendhost:80/"
    ProxyPassReverse "/" "http://backendhost:80/"

    ## trance1 - trace8, debug
    # LogLevel debug
    ## off/on
    # TraceEnable off
</VirtualHost>
```

### configure reverse proxy for service with TLS

To access a TLS backend you need to add `SSLProxyEngine on` to the vhost.

```shell
<VirtualHost *:80>
    ServerName backend.example.de
    ServerAlias backend.example.de

    ErrorLog /var/log/apache2/reverseproxy-error_log
    CustomLog /var/log/apache2/reverseproxy-access_log combined

    ProxyPass "/" "https://backendhost:443/"
    ProxyPassReverse "/" "https://backendhost:443/"

    SSLProxyEngine on

    ## trance1 - trace8, debug
    # LogLevel debug
    ## off/on
    # TraceEnable off
</VirtualHost>
```

### add tls certificates to reverse proxy


### Connect to ldap

You need to trust cert of ldap if it uses TLS. Add the following tp  apache2.conf:

```shell
LDAPTrustedGlobalCert CA_BASE64 "/etc/ssl/CA_cert.pem"
```

```shell
<VirtualHost *:80>
    ServerName backend.example.de
    ServerAlias backend.example.de

    ErrorLog /var/log/apache2/reverseproxy-error_log
    CustomLog /var/log/apache2/reverseproxy-access_log combined

    ProxyPass "/" "https://backendhost:443/"
    ProxyPassReverse "/" "https://backendhost:443/"

    # use Location instead of Proxy for static files
    <Proxy *>
        AuthType Basic
        AuthName "Enter LDAP credentials"
        AuthBasicProvider ldap

        AuthLDAPSubGroupClass group
        AuthLDAPGroupAttributeIsDN On

        AuthLDAPURL "ldaps://{{example.ldap.host}}:{{example.ldap.port}}/DC=example,DC=de?uid?sub?(objectClass=*)"

        AuthLDAPBindDN CN={{example.ldap.user}},OU=People,DC=example,DC=de
        AuthLDAPBindPassword {{example.ldap.password}}

        require ldap-group cn={{item.ldapRole}},ou=wasgruppen,ou=rechte,dc=example,dc=de

    </Proxy>

    ## trance1 - trace8, debug
    # LogLevel debug
    ## off/on
    # TraceEnable off
</VirtualHost>
```

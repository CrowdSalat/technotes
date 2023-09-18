# How to test a HTTP/HTTPS URL with a given IP address

```shell
# http only
curl --header 'Host: a.example' https://x.example

# https/SNI
curl --head --insecure --resolve a.example:443:b.example https://a.example
curl --head --insecure --resolve a.example:443:192.168.1.1 https://a.example

# or
curl --head --insecure --connect-to a.example:443:x.example:443 https://a.example
curl --head --insecure --connect-to a.example:443:192.168.1.1:443 https://a.example
```

[Source](https://stackoverflow.com/questions/50279275/curl-how-to-specify-target-hostname-for-https-request)

# Registry

## trust cert of registry without trusting it 

Place the ca certificate inside /etc/docker/certs.d/<docker registry>/ca.crt. Inlcude the port number in the path if you specify it when pulling. E.g: `/etc/docker/certs.d/my-registry.example.com:5000/ca.crt`

Get the cert in browser or with:  
 
```
openssl s_client -showcerts -connect myregistry.de:443
```

## ignore cert of registry

Edit /etc/docker/daemon.json and add insecure-registries.
  
```json
{
    "insecure-registries" : ["myregistry.de:443", "myregistry.de"]
}

```
  
  Restart docker: `sudo systemctl restart docker`

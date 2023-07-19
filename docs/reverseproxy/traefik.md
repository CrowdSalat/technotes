# Traefik

## components

- [Providers](https://doc.traefik.io/traefik/providers/overview/) discover the services that live on your infrastructure (their IP, health, ...)
- [Entrypoints](https://doc.traefik.io/traefik/routing/entrypoints/) listen for incoming traffic (ports, ...)
- [Routers](https://doc.traefik.io/traefik/routing/routers/) analyse the requests (host, path, headers, SSL, ...)
- [Services](https://doc.traefik.io/traefik/routing/services/) forward the request to your services (load balancing, ...)
- [Middlewares](https://doc.traefik.io/traefik/middlewares/overview/) may update the request or make decisions based on the request (authentication, rate limiting, headers, ...)

## static vs. dynamic config

Configuration in Traefik can refer to two different things:

- The fully dynamic routing configuration (referred to as the [dynamic configuration](https://doc.traefik.io/traefik/getting-started/configuration-overview/#the-dynamic-configuration))
- The startup configuration (referred to as the [static configuration](https://doc.traefik.io/traefik/getting-started/configuration-overview/#the-static-configuration))

## activate dashboard 

- [insecure](https://doc.traefik.io/traefik/operations/dashboard/#insecure-mode)
- [with auth ](https://doc.traefik.io/traefik/operations/dashboard/#secure-mode)

## simple reverse proxy example

static conf

```yaml
entryPoints:
  web:
    address: ':80'
providers:
  file:
    filename: /path/to/dynamic/conf.yaml
```

dynamic conf (reverse-proxy)

```yaml
# /path/to/dynamic/conf.yaml
http:
  routers:
    my-router:
      rule: PathPrefix(`/`)
      service: my-service
      middlewares:
        - stripPath
  middlewares:
    stripPath:
      stripPrefix:
        prefixes:
          - "test"
        forceSlash: false
  services:
    my-service:
      loadBalancer:
        servers:
          - url: 'http://localhost:8080'
```

[Source](https://stackoverflow.com/questions/60227270/simple-reverse-proxy-example-with-traefik)

## systemd service file

Can be found [here](https://github.com/traefik/traefik/blob/master/contrib/systemd/traefik.service).

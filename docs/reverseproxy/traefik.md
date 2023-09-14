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

## TLS

- HTTPs is configured at the router level (so in dynamic config) (routers.<name>.tls=true)
- TLS certs can be provided in a seperate dynmic config file
- Traefik searches though certificates (tls.certificates) for the right cert
- When no cert is found it serves a default certificate
- You can override the default certificate by creating a certificate store named default

[Traefik Proxy 2.x and TLS 101](https://traefik.io/blog/traefik-2-tls-101-23b4fbee81f1/)

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
  # examples for path resultin blue box "Behavior examples": https://doc.traefik.io/traefik/middlewares/http/stripprefix/#forceslash
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

## rewrite location header (after redirect)

If a server behind traefik send an redirect (302) you may need to intercept it otherwise the domain or the protocol can be switched (like with proxy_redirect in nginx or ProxyPassReverse in apache)

Possible solutions:

1. Use RedirectRegex Middleware (caveats browser sees second redirect and might create a loop)
2. use [traefik-plugin-rewrite-headers traefik plugin](https://plugins.traefik.io/plugins/628c9eb5108ecc83915d7758/rewrite-header) (supposedly breaks websockets)
3. There is an [open issue in traefik](https://github.com/traefik/traefik/issues/5809) to support it out of the box

[Source](https://stackoverflow.com/questions/58536983/can-traefik-rewrite-the-location-header-of-redirect-responses-302)

## chain of proxies

You might want to forward existing X-Forwarded-* Headers or create some of you own, when they are not there.

1. Traefik creates The X-Forwarded-* Headers by default. The list of created X-Forwarded headers can be found [here](https://doc.traefik.io/traefik/getting-started/faq/#what-are-the-forwarded-headers-when-proxying-http-requests).
2. To keep existing X-Forwarded Headers you need to configure trusted IPs on the endpoint ([forwardedHeaders.trustedIPs](https://doc.traefik.io/traefik/routing/entrypoints/#forwarded-headers))

# DNS

## dig

```shell
# see which nameservers are involved
dig weyrich.dev +trace

# see information on domain
dig weyrich.dev +all

# use specific dns to resolve url
dig @ns1-09.azure-dns.com bla.example.cloud
```

## DNS Zones

Is part of a dns tree e.g. bla.examples.com and blubb.example.com can be two different zones.

## record types

- NS: Name Server Resource Record

## CNAME vs ALIAS

##  apex domain vs subdomain

An apex domain also called root domain: weyrich.dev
Subdomain: bla.weyrich.dev

## hint: setting ns

When dig says `BAD (HORIZONTAL) REFERRAL` you probably created a loop in a ns entry. Check if you used the correct address for nameservers of the subdomain. In doubt double check the NS addresses.

E.g. azure uses different nameserver and randomly assign four to one dns zone.

## Add domain temporarily in firefox

[Firefox plugin to add temp domains](https://addons.mozilla.org/de/firefox/addon/livehosts/)

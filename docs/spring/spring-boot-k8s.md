# Spring boot on k8s

- [Official guide spring boot on k8s](https://spring.io/guides/topicals/spring-on-kubernetes/)

- Allow the jvm to sync memory consumption with k8s
- sync properties at runtime


## Graceful shutdown

[Graceful shutdown](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-graceful-shutdown)

```properties
server.shutdown=graceful
spring.lifecycle.timeout-per-shutdown-phase=15s
```

## Add readiness and liveness probes

[Add readiness and liveness probes](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes)

- add spring-boot-starter-actuator and
- add spring-boot-starter-web

Configure in k8s manifest yaml:

```
livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: <actuator-port>
  failureThreshold: ...
  periodSeconds: ...

readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: <actuator-port>
  failureThreshold: ...
  periodSeconds: ...
```

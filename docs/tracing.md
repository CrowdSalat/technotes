# Tracing

## TODO

see you obsidian notes (2025-01-07)

## with spring boot 

Configure it:

```yaml
spring.application.name: micrometer-tracing
# url of collector
# port 4318 is http but there are more
management.otlp.tracing.endpoint: http://jaeger:4318/v1/traces
management.tracing.sampling.probability: 1.0

```

Add the following dependecies

```xml
<!-- Adds the Tracing API -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-tracing</artifactId>
</dependency>
<!-- Adds the Tracer Implementation -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-tracing-bridge-otel</artifactId>
</dependency>
<!-- Adds an exporter to store the traces -->
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-exporter-otlp</artifactId>
</dependency>
<!-- Only neeeded for autconfiguration if feign is used as rest client. Otherwise traceid is not send -->
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-micrometer</artifactId>
    <version>1.0.7</version>
</dependency>
```

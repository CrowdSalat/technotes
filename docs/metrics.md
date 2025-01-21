# Metrics

## Prometheus

### promql snippets

```promql
# list metrics
{__name__!=""}

```

### Alert

There are two different types of rules: Alerts and Records. Alerts are used to monitor resources; when the Prometheus expression is fulfilled, an alarm is created. Records are used for pre-calculation of expressions. 

You need AlertReciver if you want to send an E-Mail or Notify via other way [see](https://docs.openshift.com/container-platform/4.10/monitoring/managing-alerts.html#configuring-alert-receivers_managing-alerts).

Example of alert:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: prometheus-rule-with-alert
spec:
  groups:
  - name: general.rules
    rules:
    - alert: example
      annotations:
          description: 'Shown in OCP'
          summary: "might be used to describe promql"
      expr: count(kube_job_status_failed{job_name=~'.*'}==1) > 2
      for: 60m
      labels:
          severity: warning
```

## Grafana

See you github Repo how to use operator to install grafana and configure datasources.

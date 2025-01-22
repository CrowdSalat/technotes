# Metrics

## k8s components

- Components:
    - [metric-server](https://github.com/kubernetes-sigs/metrics-server) - Is used for vertical and horizontal pod autoscaler and should not be used for other purposes. Only in memory, no history.
    - [kube-state-metrics (KSM)](https://github.com/kubernetes/kube-state-metrics/) - expose metrics. Is used by prometheus. Only in memory, no history.
    - [prometheus](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus)
- interaction of components:
    - metric-server (in memory) is used to collect metrics 
    - kubelet: exposes the metrics of the node (and all its pods) which can then be collected by the metric server
    - cAdvisor is the subcomponent of the kubelet who collect the metrics from the pods
- metrics-server can be deployed with: `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`

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

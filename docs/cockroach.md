# minimal notes on cockroach

- distributed relational database (acid)
- transparent sharding (which can be controlled if needed: [Topology Patterns](https://www.cockroachlabs.com/docs/stable/topology-patterns.html))
- in contrast to postgres it does not use active-passive replication (uses quorum)
- CAP theorem: is a cp database
- uses postgres "interface": ORM, SQL dialect, etc.
- runs on kubernetes
- serverless offer in cloud (uses k8s under the hood)

# Logging stacks

## k8s logging

distinct between:

- Container logs
- Kubernetes system components logging which run on the node itself (typically only kubelet, but may be more)


- container log to stdout which is collected by k8s and can be viewed via kubectl
- container logs are stored at kubelet more details [here](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- Options for collecting container logs:
1. logging sidecar container running inside an app’s pod.
2. using a node-level logging agent that runs on every node.
3. push logs directly from within an application to some backend.

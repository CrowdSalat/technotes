# Logging stacks

## k8s logging

distinct between:

- Container logs
- Kubernetes system components logging which run on the node itself (typically only kubelet, but may be more)

Options for collecting container logs:

- logging sidecar container running inside an app’s pod.
- using a node-level logging agent that runs on every node.
- push logs directly from within an application to some backend.

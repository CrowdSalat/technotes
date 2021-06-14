# Kuberentes overview

Kubernetes (K8s) allows to deploy multiple containers on one or multiple host systems. K8s is essentially an API for a control loop (controller) which constantly checks if a resources is in a desired state and reconcile its state if it is not the case. The [official documentation of k8s](https://kubernetes.io/docs/) is outstanding. There is also a [interactive tutorial](https://kubernetes.io/docs/tutorials/kubernetes-basics/). 

Outline of features:

- one master node and one/multiple worker nodes
- creates self-healing clusters 
- allows rolling upgrade and rollback
- secret management and encapsulation via namespaces

- [Reference](https://kubernetes.io/docs/reference/)
- [glossary](https://kubernetes.io/docs/reference/glossary/?fundamental=true)
- 


## Components 

- control plane components (manager)
  - kube-apiserver - accepts http request of clients and can be directly accessed by a client library like [kubectl](https://kubernetes.io/docs/reference/kubectl/)
  - etcd - saves state of workers
  - kube-scheduler - placement of pod based on load and other factors
  - kube-controller-manager - keep track of state on workers
- node components (worker)
  - kubelet - kubernetes node agent
  - kube-proxy - manages networking for node
  - container runtime - containerd compliant software

### kubectl

- [kubectl cli ref](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
- [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [Deep dive into api structure](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md)
- [Imperative commands](https://kubernetes.io/docs/concepts/overview/working-with-objects/object-management/)

## Objects

Kubernetes objects:

- represent the state of your cluster
- describes the desired state
- k8s will try to keep them alive

Most have the following two fields:

- spec: desired state (which is described by you when creating the object)
- status: current state (which is updated and managed by control plane)

Usually described in a .yaml file:

- which can be used with `kubectl apply`. 
- which always contains at least the following fields: 
  - `apiVersion`: version of Kubernetes API you're using to create this object
  - `kind`: - kind of object you want to create
  - `metadata`: - Data that helps uniquely identify the object (a name string, UID, and optional namespace)
  - `spec`: - desired state for the object
- the fields for every kind of object are described [here](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/).

**the yml file will get translated to a JSON which is send as a http payload to the apiserver**

### Pods

- are one or multiple containers which
- share a unique network IP Address
- "Containers should only be scheduled together in a single Pod if they are tightly coupled and need to share resources such as disk."

### Controllers

Controllers allow:

1. fail over
2. scaling
3. load balancing

Kind of controllers:

- RepilcaSet: ensures that a specified number of pods is runnning in a node (needs another controller as wrapper like deployment)
- Deployment: declarative describe how to create and update instances (self-healing mechanism)
- DaemonsSets: ensures that all nodes run a specific copy of a pod (1 DaemonSet - 1 Pod - X nodes).
- Jobs: supervisor for pods which do batch jobs
- Services: allow communication between deployment controllers

### Services

- allows to manage how the pods can be accessed
- defines a logical set of pods 
- pods are selected via a LabelSelector 
- modes:
    - ClusterIP (default): pods can only communicate within k8s cluster over there IP
    - NodePort: pods can be accessed through the node ip and the so called NodePort
    - load balancer: to expose application to the internet
    - ExternalName: expose a service by a name


### Labels

- added to objects like pods, services, deployments
- key-value pairs to identify attributes of objects

### Selectors

- allows to select objects by a label
- equality-based: `=` and `!=`
- set-based: `IN`, `NOTIN`, `EXISTS`
  
### Namespaces 
- allows multiple virtual clusters on the same physical host
- there is a default namescpace

## install

- Install Docker like in the [official documentation](https://docs.docker.com/engine/install/) or use the install script `curl -sSL https://get.docker.com | sh` which might be easier
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- install [auto completion for kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enable-kubectl-autocompletion): `kubectl completion bash >/etc/bash_completion.d/kubectl` *might get permission issues with this command. Than write it  to ~/kubectl and mv it with sudo to the target folder*
- if you want to test stuff on your local machine  [install minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/#before-you-begin). **Note the 'Before you begin' section which shows you how to check for a supervisor.** If you do not have one install one like VirtualBox.

## kubectl cli

[kubectl reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
[cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)


basic commands:
- `kubectl cluster-info` list main nodes and worker nodes
- `kubectl apply -f FILENAME` create/updates the defined objects in the given yaml file.
- `kubectl get all`
    - `kubectl get nodes`
    - `kubectl get services`
    - `kubectl get deployments`
    - `kubectl get rs` list replica sets
    - `-l <label>` to load only resources with the given label

update pod:
- `kubectl set image deployments/<POD_NAME> <POD_NAME>=<IMAGE>:<VERSIONTAG>` set image of a pod to a newer version
- `rubectl rollout undo deployments/<POD_NAME> ` rollback to previous version

create resources without k8s file:
- `kubectl run <POD_NAME> --image=<IMAGE>` starts a pod with the given image
- `kubectl create deployment <deployment_name/POD_NAME> --image=<IMAGE>` creates a deployment which starts a pod with the given image
- `kubectl expose <POD_NAME> --type=NodePort --port=80` creates a service on port 80 of the node.
- `kubectl label pod <POD_NAME> <labelKey=labelValue>` creates a label on a pod
- `kubectl scale deployments/<POD_NAME> --replicas=4` creates a replication set

debug pod:
- `kubectl proxy ` proxy into a cluster even when no services are exposed. Pods are accessible via there pod name or there internal ip address. Terminate with ctrl + c.
- `kubectl describe <node/pods/deployment>`
- `kubectl kubectl logs <POD_NAME>`
- `kubectl exec <POD_NAME>` - execute a command on a container in a pod
- `kubectl exec -ti <POD_NAME> bash` open bash in container

## minicube cli

- `minicube start`
- `minicube service <POD_NAME>` shows the service in the browser

## k8s yaml file (manifests)

[Overview](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

Required fields:

- apiVersion: kubernetes API version you use
- kind: type of object (e.g. Deployment, Pod)
- metadata: fields that allow to uniquely identify the object (name, UID, optional namespace)
- spec: the state you desire for the object. The spec fields are different for ever object and can be look up under [kubernetes api reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#container-v1-core).

## Operators

- An operator is a custom controller which operates on a whole application (e.g. kafka, mysql) instead on an object (Pod, Service, ConfigMap..)
- An API build for running a specific application in k8s. 
- It is build on top of k8s resource and controller api.
- only needed for stateful applications
- extends k8s API (can be used like other resources in a manifest file)

- The [article which presented operator](https://coreos.com/blog/introducing-operators.html) has a pretty good FAQ
- There is a [maturity model for opertaors](https://docs.openshift.com/container-platform/latest/welcome/index.html)
- There is a currated list of Operators in [this repository](https://operatorhub.io/)
- To build one use [Operator SDK](https://github.com/operator-framework/operator-sdk/tree/v0.11.0)

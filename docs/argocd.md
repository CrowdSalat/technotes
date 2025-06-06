# Argocd

## argo projects

if you use argo project != default you might need to add additional configuration to them e.g. "Source Repositories" or "Destinations" in order that the argo applications in the project work as expected. To make it work you can also use "*" for these configurations, but I would not recommend it. Better to understand it correctly and set specific values, have a look at  [Argo official Project doc](https://argo-cd.readthedocs.io/en/latest/user-guide/projects/)

To see this in UI: Settings -> Projects

## Self-signed & Untrusted TLS Certificates

If a git repository uses a self signed certificate you might see the exception "failed to verify certificate x509 certificate signed by unknown authority" when creating a argo app.

You can add the certficate via settings, for details [see official doc](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/#managing-tls-certificates-using-the-argocd-web-ui). 

It seems that the easiest way is to add the root ca. It seems that if the CN is not correct for the "Repository Server Name" the validation fails in argo (but to be honest I am not entirely sure). Double check that you have written the exact "Repository Server Name" that was shown in the exception. 

## credential template (for git repositories)

Based on [official argocd doc](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#repository-credentials)

**beware when using ssh: ingress and routes do not support ssh over port 22, only https**

credential template allow that you connect to a git repository without defining the credentials.

The URL configured for a credential template (e.g. https://github.com/argoproj) must match as prefix for the repository URL in an Argo application. A credential template might be overriden by the credentials in a "repository" config of argo.

Steps:

1. Add git server to known host by appending an existing configmap (ssh only)
2. Check if known host is picked up in UI: Settings -> Repository certificates and known hosts (ssh only)
3. Create the credential template by creating a secret 
4. Check if credential template is picked up in UI: Settings -> Repositories
5. profit: use this template in your argo application


### 1. Add git server to known host by appending an existing configmap


```shell
# copy the content of 
ssh-keyscan URL_OF_MY_GIT_SERVER

# to 
oc edit -n openshift-gitops configmap argocd-ssh-known-hosts-cm
```

### 3. Create the credential template by creating a secret 

```yaml
kind: Secret
apiVersion: v1
metadata:
  name: git-repo-credential-template
  namespace: openshift-gitops
  labels:
    # needed to be used by argo 
    argocd.argoproj.io/secret-type: repo-creds
data:
  # echo -n "git" | base64
  type: Z2l0Cg==
  # cat id_ed25519 | base64
  sshPrivateKey: PLACEHOLDER
  # for ssh: echo -n "git@gitlab.example" | base64
  # for https: echo -n "https://gitlab.example" | base64
  url: PLACEHOLDER
type: Opaque

```

Or if you want to create the yaml with cli:

```shell
# generate Repository template for ArgoCD
oc create secret generic git-repo-template --from-file=sshPrivateKey=./key --from-literal=url=git@gitlab.example --from-literal=type=git -o yaml --dry-run=client > argo-repositorytemplate-secret.yaml 

## then add label so it is used by argo: 
# argocd.argoproj.io/secret-type: repo-creds
code argo-repositorytemplate-secret.yaml 

# 
oc apply -f argo-repositorytemplate.secret.yaml -n openshift-gitops
```

#### or for https instead of ssh

Tbh a bit less failure prone while creating and also no issues when you host git server via K8s/OpenShift with Ingress/Route

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo-creds
  namespace: openshift-gitops
  labels:
    argocd.argoproj.io/secret-type: repo-creds
stringData:
  type: git
  url: https://github.com/argoproj
  password: my-password
  username: my-username
```

## example application

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: test
spec:
  destination:
    server: 'https://kubernetes.default.svc'
  project: default
  source:
    path: .
    repoURL: 'https://gitlab.apps.ocp.weyrich.dev/gitops/vms.git'
    targetRevision: HEAD
```
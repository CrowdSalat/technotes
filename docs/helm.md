# Helm

[Helm](https://helm.sh/) is a package manager for k8s (like apt, yum, pip, ...)

- a **chart** is a package in helm which contains all resources for running an application
- a **repository** is a source for charts (by default [artifacthub](https://artifacthub.io))
- a **release** is an instance of a running chart

## cli overview

```shell
# generate k8s manifest file from the helm chart in the current directory
helm template <my_release_name> . > manifest.yml

# directly apply helm chart from a repository to k8s cluster
helm install <my_release_name> <chart_name>
# delete helm-release from cluster
helm uninstall <my_release_name>

# overwrite variables with values file
helm template <my_release_name> -f values.yaml .

# overwrite variables on cli
helm template <my_release_name> -f values.yaml --set overwriteKey=overwriteVal .

#search chart in public repository
helm search hub <chart_name>

# if need be you can add the old repository (helm1 and helm2 chart) from https://github.com/helm/charts
helm repo add deprecated_incubator https://charts.helm.sh/incubator/
helm repo add deprecated_stable https://charts.helm.sh/stable/
#search chart in your locally added repositories
helm search repo <chart_name>

# list default config of a helm chart
helm show values <chart_name>

# overwrite default config of helm chart
helm install -f config.yaml <chart_name>

# list installed helm-releases
helm list
# uninstall helm-release
helm uninstall <my_release_name>

# download helm sources of chart to local folder
helm pull <chart_name> --untar

# show manifest of a installed release tmp
helm get manifest tmp

# show values of a installed release tmp
helm get values tmp
```

## umbrella chart pattern

- one single root chart.yml
- connection to other charts via dependencies
- one value file to overwrite sub charts
- once to initiate run: `helm dep up`
- to support multiple environments or tenants you can add additional value.yaml files

## extend content of fields

It is easiert to extend a map than a list. In short, you can not extend/merge lists which is defined in a another values.yaml. A list is overriden by default. A map entry can be overriden by its name. Or the map is extended if you use a new entry field name.

## local dependecy

The version and the chart name need to match event when the reference is locally.

In Chart.yaml
```yaml
...

dependencies:
  - name: your-dependency-name
    version: "1.2.3"
    repository: "file://../path/to/your/local/chart"
```

## notes on certificates

Not all cli command support ignoring or setting ca certs (*e.g. `helm dependency update` does not*) so you might consider to add the needed certificates system wide on os/container level.

## snippets

[Helm template guide](https://helm.sh/docs/chart_template_guide/) gives a quick overview of the flow control and the available template functions. Under the hood helm uses a mix of [Go template language](https://pkg.go.dev/text/template?utm_source=godoc) and [Sprig template library](https://masterminds.github.io/sprig/).

### required values


If you want to check at the top of the file without rendering it to the template:
```yaml
{{ $_ := required "A valid .Values.who entry required!" .Values.who}}
{{ $_ := required "A valid .Values.who entry required!" .Values.who2}}

...

value: {{ .Values.who }}
```

or inline

```yaml
value: {{ required "A valid .Values.who entry required!" .Values.who }}
```

### load file

``` yaml

spec:
  inlineMultilineField: |
{{ tpl (.Files.Get "relativePath/file.json" ) . | indent 4 }}

```

### doc in values.yaml

see [best practices](https://helm.sh/docs/chart_best_practices/values/#document-valuesyaml)
 
```yaml
# serverHost is the host name for the webserver
serverHost: example
# serverPort is the HTTP listener port for the webserver
serverPort: 9191
```

### range

- [Different examples with result](https://kb.novaordis.com/index.php/Helm_Template_range)
  - iterate over inline array
  - iterate over array
  - iterate over dictionary/map
  - iterate over complex type

```go
// count loop
{{- range  until (.Value.upperBound | int) }}
    count: {{ . }}
{{- end }}

```

**If you want to access all values (not only the looped on) inside of range, you need to access the global scope e.g. $.Values.bla**

### compare numbers (eq, ne, lt, gt, le, ge)

If you want to compare a value with a integer literal it might say that you cannot compare types. In this case add a decimal place to it.

```go
// change
{{- if eq .Value.number 1 }}
// to
{{- if eq .Value.number 1.0 }}
```

### access dotted values

```yaml
# values.yaml
ca.password: bla

---

# template
apiVersion: v1
kind: Secret
metadata:
  # variable usage
  name: {{ index .Values "ca.password" }}
....
```

### variables

```shell
# variable declaration and initiation
{{- $random := ( randAlphaNum 5 | quote) }}
apiVersion: v1
kind: Secret
metadata:
  # variable usage
  name: name-{{ $random }}
....
```


### runtime values

Runtime values can be accessed with the [lookup](https://helm.sh/docs/chart_template_guide/functions_and_pipelines/#using-the-lookup-function) function. The lookup values will only be visible when calling install. Helm template will not substitute these values. It might not work with argocd, because argo uses helm template and kubectl apply and not helm install.

You can find an nice example how to lookup a password [here](https://codersociety.com/de/blog/articles/helm-best-practices#11-use-the-lookup-function-to-avoid-secret-regeneration).

### trigger reload of resource when connfigmap or secret changes

```yaml
kind: Deployment
spec:
  template:
    metadata:
      annotations:
        checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}
```
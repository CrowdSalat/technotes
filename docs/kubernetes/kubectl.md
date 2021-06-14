# kubectl snippets

## secrets

```shell
# copy secret from one namespace to another
kubectl get secret "secretName" --namespace=bla -oyaml | grep -v '^\s*namespace:\s' | kubectl apply --namespace=blubb -f -

# download key of secret to file
kubectl get secret "secretName" -o jsonpath='{.data.secretKey}' | base64 --decode > content.txt
```
# Pulumi

## login to cloud provider

### azure

Pulumi runs against the default azure subscription. If you got authentifcation errors you might want to check if you selected the right subscription.

```shell
az login
az account list
az account set --subscription=<id>
```

## pulumi print values

One option is to define [output properties](https://www.pulumi.com/learn/building-with-pulumi/stack-outputs/) and then run `pulumi stack output`. You can define output in Typescript by exporting a variable like this `export const someId = resource.id`

## string interpolation

Use pulumi.concat or pulumi.interpolate. For deails [see](https://www.pulumi.com/docs/intro/concepts/inputs-outputs/#outputs-and-strings)

## pulumi native vs. classic

In doubt use native because.

- it exposes the full api of a provider (beware of breaking api changes)
  - in case of azure it only exposes the ARM api which is a (unified) subset of the rest api [(see)](https://github.com/pulumi/pulumi-azure-native/issues/742#issuecomment-909352006)
- it allows to configure the location for the whole stack: `pulumi config set azure-native:location WestUS2`


The classic versions are based on the terraform providers.

[Pulumi blog](https://www.pulumi.com/blog/full-coverage-of-azure-resources-with-azure-native/)

## pulumi flow for pulumi native

- create in web ui
- see template of ressource
  - azure: "Automation" > "Export template"
- you can copy part of the stuff to pulumi

## refresh state

If you got diff between pulumi state and actual state.

```shell
pulumi up --refresh
pulumi refresh
```
## see diff

`pulumi preview -diff` or `pulumi up -diff`

## use custom backend

- setup with standard login. The initial project nearly setups an backend. Run pulumi up and then switch the backend to newly created resources. 
- otherwise here is a gist for creating the resources with a bash scripts
  - [azure gist](https://gist.github.com/CrowdSalat/07a07e923ed29933eaf9abe54bf1f83b)
  - [gcp gist](https://gist.github.com/CrowdSalat/07a07e923ed29933eaf9abe54bf1f83b)
- Login to [selfmanaged backend](https://www.pulumi.com/docs/intro/concepts/state/#using-a-self-managed-backend)
  - azure: 
    - set AZURE_STORAGE_KEY and STORAGE_ACCOUNT_NAME
    - run `pulumi login azblob://<container-blob-name>`
  - gcp:
    - set GOOGLE_PROJECT and GOOGLE_CREDENTIALS
    - run  `pulumi login gs://<my-pulumi-state-bucket>`

## access stack name within code

In doubt keep stacknames **short** and the **same size** when you want to add it to the resources. So you can be sure that length requirements of resource names are fullfilled in every stack and not just in a few.

```typescript
let stack = pulumi.getStack();
```

Source: [https://www.pulumi.com/docs/intro/concepts/stack/#getting-the-current-stack-programmatically](https://www.pulumi.com/docs/intro/concepts/stack/#getting-the-current-stack-programmatically)

## stack dependant variables

Add an entry under config in the config-<stack>.yaml in root.

```typescript
let config = new pulumi.Config();
let name = config.require("name");
```

Source: [Accessing Configuration from Code](https://www.pulumi.com/docs/intro/concepts/config/#code)

## see api of existing resource

### azure

az resource.. list

```shell
az containerapp list
az containerapp env list
az monitor log-analytics workspace list
```

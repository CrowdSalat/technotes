# Pulumi

## use custom backend

- setup with standard login. The initial project nearly setups an backend. Run pulumi up and then switch the backend to newly created resources. 
- otherwise here is a gist for creating the resources with a bash scripts
    - [azure gist](https://gist.github.com/CrowdSalat/07a07e923ed29933eaf9abe54bf1f83b)
    - [gcp gist](https://gist.github.com/CrowdSalat/07a07e923ed29933eaf9abe54bf1f83b)
- Login to [selfmanaged backend](https://www.pulumi.com/docs/intro/concepts/state/#using-a-self-managed-backend)
  - azure: 
    - set AZURE_STORAGE_KEY and STORAGE_ACCOUNT_NAME
    - run `pulumi login azblob://<container-path>`
  - gcp:
    - set GOOGLE_PROJECT and GOOGLE_CREDENTIALS
    - run  `pulumi login gs://<my-pulumi-state-bucket>`

## access stack name within code

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

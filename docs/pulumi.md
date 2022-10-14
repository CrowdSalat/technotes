# Pulumi

## use custom backend

- setup with standard login. The initial project nearly setups an backend. Run pulumi up and then switch the backend to newly created resources. s
- otherwise here is a gist for creating the resources with a bash scripts
    - azure
    - gcp


## linter for pulumi and save actions
## gitops for pulumi

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

# Azure

## orientation

- information on service: Look into the FAQ
- automatic analysis: advisor recommendations
- create graphic: Resource Visualizer (part of Resource Group)
- IaC:
  -  Azure Resource Manager templates (ARM templates)
  -  in most resources under 'Export template'
  -  can be used to be copie to pulumi azure native provider
- audit and see erros in deployment: Monitor | Activity log

## terms

- directory: company
- subscription: part of a directory
- account: me and belongs to one or more directory and has access to one or more subscriptions

```shell
az login
az account list
az account set --subscription=<id>
```

## collect logging in resorce group 

If you want to access all logs on resource group level you need to create a log analytic workspace.

## Container apps vs container instances

Use container apps when you

- need autoscaling
- want easy loadbalancer integration (each app separately) with azure domain (or custom domain)

[Source](https://learn.microsoft.com/en-us/azure/container-apps/compare-options)

## Postgres single server vs flex

Flexible server

- recommended service according to azure portal
- newer postgres versions available
- zone replication

[Source](https://learn.microsoft.com/en-us/azure/postgresql/single-server/overview-postgres-choose-server-options)

## blob vs container

Blob are grouped in containers. Containers belong to storage accounts.

## list region/location codes

Run `az account list-locations -o table` or look [here](https://stackoverflow.com/a/56309831)

## create service account for pulumi/terraform in pipeline:

See [here](https://gist.github.com/CrowdSalat/07a07e923ed29933eaf9abe54bf1f83b) or:

```shell
#!/usr/bin/env bash

set -e

SUBSCRIPTION=
NAME=XXXX-pipeline-service-principal
YEARS=10

az ad sp create-for-rbac --name="${NAME}" --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION}" --years=$YEARS --sdk-auth > .localsecret_pipeline

## to get information on user
# az ad sp list --display-name=${NAME}

## reset password and print it (if old password is lost)
# az ad sp credential reset --name=${NAME}

## delete service principal
# az ad sp delete --id 00000000-0000-0000-0000-00000000000000000
```

## managed identities

[Managed identities](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview) are an alternative to credentials. They are bound to a resource and allow the resource to interact with other resources with the rights of the identity (similar to aws roles). 

- System-assigned: identities are created by azure and the lifecycle is bound to the resource in which it was created (e.g. the container app)
- User-assigned: configure a service principal for the resource. Lifecycle is controled by user.

## azure exctensions

Some commands `az containerapp` do not run out of the box. You need to install the extension to use it.

```shell
# list extensions and see if they are installed
az extension list-available --output table

# add containerapp extension
az extension add --name containerapp
```

[Official doc](https://learn.microsoft.com/en-us/cli/azure/azure-cli-extensions-overview)

## see arm template of resource with az

```
az containerapp connection list
```

## app registration vs enterprise app / service principal

[blog article](https://marileeturscak.medium.com/the-difference-between-app-registrations-enterprise-applications-and-service-principals-in-azure-4f70b9a80fe5)
[Create service principal in UI](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal?WT.mc_id=AZ-MVP-5004737)

## Container apps comunication

Online as well as communication between to container apps need to use the ingress. You can choose if ingress is public available or only for other container apps.

The url is: CONTAINER_APP_NAME.CONTAINER_APP_ENV_NAME.REGION_NAME.azurecontainerapps.io
E.g. can be look like this: app.kindflower-050d0634.germanywestcentral.azurecontainerapps.io

Every container app revision got an URL as well. This URL contains the revision name as subdomain instead of the conainter app name.

## container app debugging

It may happen that a container is killed but you got no log entry other than "killed".

Troubleshooting:s set the memory and cpu to max (or check in metrics if the memory metric is too high before it collapesesz


## pipelines user for az

[How to create and use](https://github.com/Azure/login#configure-deployment-credentials)

`az ad sp create-for-rbac --name="myapp-service-principal" --role="Contributor" --scopes="/subscriptions/BLA" --years=10 --sdk-auth`

Create sp with `--sdk-auth` flag so your output looks something like this. Action does not work with the normal outoput of four properties.

```json
 {
 "clientId": "",
 "clientSecret": "",
 "subscriptionId": "",
 "tenantId": "",
 "activeDirectoryEndpointUrl": "",
 "resourceManagerEndpointUrl": "",
 "activeDirectoryGraphResourceId": "",
 "sqlManagementEndpointUrl": "",
 "galleryEndpointUrl": "",
 "managementEndpointUrl": ""
 }
 ```


Pipeline example:

```yaml
name: Deploy Backend feat

on:
  push:
    branches:
      - azure-env
    paths:
      - 'graphql_backend/**/*'
      - '.github/workflows/azure-feat-backend.yml'
env:
  AZURE_REGISTRY_USERNAME: myapp
  REGISTRY: myapp.azurecr.io
  REPOSITORY: myapp-2834-repo
  IMAGE_TAG:  backend-${{github.sha }}
  AZURE_RESOURCE_GROUP: dev-myapp322577c2
  AZURE_CONTAINER_APP_NAME: dev-myapp-app-backend

jobs:
  build:
    name: Build Backend
    runs-on: ubuntu-latest
    environment: dev
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - uses: azure/docker-login@v1
        with:
          login-server: ${{ env.REGISTRY }}
          username: ${{ env.AZURE_REGISTRY_USERNAME }}
          password: ${{ secrets.AZURE_REGISTRY_PASSWORD }}

      - name: Build, tag, and push Backend container registry
        id: build-image-backend
        run: |
          docker build -t ${{ env.REGISTRY }}/${{ env.REPOSITORY}}:${{ env.IMAGE_TAG }} graphql_backend
          docker push ${{ env.REGISTRY }}/${{ env.REPOSITORY}}:${{ env.IMAGE_TAG }}
          echo "::set-output name=image::${{ env.REGISTRY }}/${{ env.REPOSITORY}}:${{ env.IMAGE_TAG }}"    

  deploy:
      name: Deploy Backend
      runs-on: ubuntu-latest
      environment: dev
      needs: build
      steps:
        - name: Azure Login
          uses: azure/login@v1
          with:
            creds: ${{ secrets.AZURE_CREDENTIALS }}

        - name: Deploy to containerapp
          uses: azure/CLI@v1
          with:
            inlineScript: |
              echo "Installing containerapp extension"
              az extension add --name containerapp
              az config set extension.use_dynamic_install=yes_without_prompt

              # note needed when access is already created
              echo "Configure registry"
              az containerapp registry set -n ${{ env.AZURE_CONTAINER_APP_NAME }} -g ${{ env.AZURE_RESOURCE_GROUP }} --server ${{ env.REGISTRY }} --username  ${{ env.AZURE_REGISTRY_USERNAME }} --password ${{ secrets.AZURE_REGISTRY_PASSWORD }}

              echo "Deploy ${{ env.REGISTRY }}/${{ env.IMAGE_TAG }}:${{ github.sha }}"
              az containerapp update -n ${{ env.AZURE_CONTAINER_APP_NAME }} -g ${{ env.AZURE_RESOURCE_GROUP }} --image ${{ env.REGISTRY }}/${{ env.IMAGE_TAG }}
```

## azure logs

[KQL reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)


Examples:

```
# container app 

## app log
ContainerAppConsoleLogs_CL
| project TimeGenerated,  Log_s, RevisionName_s, ContainerImage_s
| sort by TimeGenerated desc  

## kubernetes events/logs
ContainerAppConsoleLogs_CL
| project TimeGenerated,  Log_s, RevisionName_s, ContainerImage_s
| sort by TimeGenerated desc  

## example timerange relative
let endDateTime = now();
let startDateTime = ago(1h);
ContainerAppConsoleLogs_CL
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| sort by TimeGenerated desc  

```

## delete old images in a repo

[on-demand task + az acr purge](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-auto-purge#run-in-an-on-demand-task)

## brew + az + completion

```shell
# workaround no az completion in zsh
# https://medium.com/@MyDiemHo/enable-azure-cli-autocomplete-in-oh-my-zsh-93e79019a20d
autoload -U +X bashcompinit && bashcompinitsource 
source /opt/homebrew/etc/bash_completion.d/az
```
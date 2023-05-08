# GCP

## orientation

- Information on Service: Look into the FAQ
- advisor recommendations
- create graphic:
- IaC:
  - google cloud deployment mamnager (GDM) - seems to be maintained but not recommended by google (terraform is promoted instead)
  - load attributes of resource as yaml: `gcloud <serviceTier> <service> describe <resourceName>` (matches the field of pulumi native-google)
- audit and see erros in deployment:

## terms

- Organization
- Project
- Zones
- Regions

```shell
# login
gcloud auth login

# list projects in organisation
gcloud projects list

# set default project for commands
gcloud config set project <PROJECT>

# list resources (of default project or --project project)
gcloud asset search-all-resources #--projects <RPOJECT>
```

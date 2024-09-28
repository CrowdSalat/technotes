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

## roles ref

[Cloud Functions Developer](https://cloud.google.com/iam/docs/understanding-roles)

## enable api

```typescript
const apis = [
    'firestore.googleapis.com' // Firestore
];

const enabledAPIs : gcp.projects.Service[] = [];
apis.forEach(api => {
    // Enable the API
    const enabledAPI = new gcp.projects.Service(`enable-${api}`, {
        service: api,
        project: projectId
    });
    enabledAPIs.push(enabledAPI);
});


//APIs
const database = new gcp.firestore.Database(`${stack}-firestore`, {
    locationId: 'eur3',
    project: projectId,
    type: 'FIRESTORE_NATIVE'

}, { dependsOn: enabledAPIs });
```

## service account vs. principal (of service account)

https://cloud.google.com/iam/docs/best-practices-service-accounts

Add roles to service account (principal, not ressource):

Pulumi:

```typescript
// bind role to service account principal
    new gcp.projects.IAMBinding(`principal-of-sa-roleassignment`, {
        project: gcpProjectId,
        role: "roles/secretmanager.admin",
        members: ["serviceAccount:sa@gcp-projectname.iam.gserviceaccount.com"],
    });

// bind role to service account resource
     new gcp.serviceaccount.IAMBinding`principal-of-sa-roleassignment`, {
        serviceAccountId: serviceAccount.id,
        role: "roles/secretmanager.admin",
        members: ["serviceAccount:sa@gcp-projectname.iam.gserviceaccount.com"],
    });
```

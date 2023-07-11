# Azure Enterprise

## ARM Tempalte deployment

[Deploy ARM with Azure portal](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/quickstart-create-templates-use-the-portal)

## create landing zone arch with hub& spoke

[Deploying ALZ HubAndSpoke](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-ALZ-HubAndSpoke)
 
### 1. SME

- Deploylink: https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/docs/reference/treyresearch/armTemplates/es-lite.json/createUIDefinitionUri/https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/docs/reference/treyresearch/armTemplates/portal-es-lite.json
- readme: <https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/treyresearch/README.md>
- wiki: https://github.com/Azure/Enterprise-Scale/wiki/Deploying-ALZ-BasicSetup

ARM templates:

- tenantDeploymentTemplate: https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/docs/reference/treyresearch/armTemplates/es-lite.json
- uiFormDefinition: https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/docs/reference/treyresearch/armTemplates/portal-es-lite.json

#### 2. Enterprise 

- Deploylink: https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/eslzArm/eslzArm.json/uiFormDefinitionUri/https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/eslzArm/eslz-portal.json
- readme: <https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md>
- wiki: https://github.com/Azure/Enterprise-Scale/wiki/Deploying-ALZ-BasicSetup

- tenantDeploymentTemplate: https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/eslzArm/eslzArm.json
- uiFormDefinition: https://raw.githubusercontent.com/Azure/Enterprise-Scale/main/eslzArm/eslz-portal.json

#### 3. manual cli <https://github.com/Azure/Enterprise-Scale/tree/main/eslzArm#deploying-in-azure-global-regions>



## transition to azure landing zone

- [Scenario: Transition existing Azure environments to the Azure landing zone conceptual architecture](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/align-scenarios#transition-to-the-azure-landing-zone-conceptual-architecture)
- [Transition existing Azure environments to the Azure landing zone conceptual architecture](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/transition)

## move resources

- [Move resources to a new resource group or subscription](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-resource-group-and-subscription)

- resource ID: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
- is resource id unique identifier?

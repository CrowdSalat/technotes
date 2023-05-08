# Patterns

For public cloud.

## Landing zones

A landing zone is a pattern to structure you public cloud account.

*"A landing zone is a well-architected, multi-account AWS environment that is scalable and secure. This is a starting point from which your organization can quickly launch and deploy workloads and applications with confidence in your security and infrastructure environment."* ([What is a landing zone? (AWS)](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html))

[Landing zone CloudFormation tempalte](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/aws-cloudformation-template.html)

*"An Azure landing zone is the output of a multi-subscription Azure environment that accounts for scale, security governance, networking, and identity. An Azure landing zone enables application migration, modernization, and innovation at enterprise-scale in Azure."* ([What is an Azure landing zone?](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/))

- [Landing zone ARM templates](https://github.com/Azure/Enterprise-Scale)
- [Landing zone ARM templates documentation](https://github.com/Azure/Enterprise-Scale/wiki)


## Hub & Spoke

The hub and spoke pattern refers to a network topology where a central cloud service (the hub) is used to connect to multiple remote locations or users (the spokes) through a virtual private network (VPN) or direct connection. The spokes can access cloud services directly through the hub, without having to establish individual connections to the cloud provider. The hub can also provide centralized security, network, and traffic management services, which can improve the overall performance, scalability, and security of the system.

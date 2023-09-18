# AWS

## orientation

- information on service: Look into the FAQ
- general health of service: [AWS Health Dashboard](https://health.aws.amazon.com/health/status)
  - if accessed without login it allows to filter resources
  - if accessed while logged in it shows health based on your resources
- automatic analysis of resources: AWS Trusted Advisor
- create graphic:
- IaC: 
  - Cloudformation
  - use [import](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import.html) to create a cloudformation template for an existing ressoure
- audit and see erros in deployment: Monitor | Activity log

## terms

- [(AWS account) root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) is created by default when an aws account is created. Should not be used directly for creating services.
- [(IAM) Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) is an entity that represents an person or an application within your organization. Consist of name and credentials
- Groups only contain iam users, but not other groups. Users can belong to 0,1,n Groups.
- IAM identities: users, groups of users, or roles 
- [Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html): JSON documents which describe permissions for users, groups or roles
- [(IAM) Role](https://docs.aws.amazon.com/de_de/IAM/latest/UserGuide/id_roles.html) is intended to be assumable by anyone who needs it (users or applications/services). It has no credentials of its own. You get temporary credentials when you assume the role.
- access keys can be used for programmatic access
  - Access Key ID ~= username
  - Secret Access Key ~= password
- Region 
- Availabilty Zone

```shell

# login
## ~/.aws/config
## ~/.aws/credentials
aws configure

# whoami (awsaccount)
aws sts get-caller-identity
```

## login to ecr

The token is valid for 12 hours. Example:

```shell
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 934387437486.dkr.ecr.eu-central-1.amazonaws.com
```

## lambda invoke patterns

[Source](https://aws.amazon.com/de/blogs/architecture/understanding-the-different-ways-to-invoke-lambda-functions/)

- Synchronous Invokes
- Asynchronous Invokes
- Poll-based Invokes

## services overview/noteworthy

### global vs. region base services

- Identity and Access Management (IAM)
- Route 53 (DNS service)
- CloudFront (Content Delivery Network)
- WAF (Web Application Firewall)

[region based](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)

### audit security

- IAM Access Advisor (user): list service permissions of a user and when they were last used
- IAM Credentials Report (account): list user and credential status

### EC2

- An EC2 Instance is based on a **Amazon Machine Image (AMI)**
- **User data** only run once at the instance first start of an instance (can be used to execute scripts)
- **EC2 Instance Connect** allows to connect to an EC2 Instance in the browser (if Amazon Linux is used) but port 22 still needs to be open
- **ECS2 Instace role** is a link to a iam role
- **Placement groups** allows to control where EC2 Instance are started (cluster, spread, (network) partition)

Storage:

- instance store (hardware)
  - faster than EBS and EFS
  - but ephemeral storage which loses state after restart
- network attached
  - Elastic block storage (EBS) - device only
    - every EC2 instance has a root EBS which by default gets deleted on EC2 Instance deletion
    - fixed size
    - onyl accessible in the same AZ and by one EC2 instance (except for mutli-attach EBS which allows n in the same AZ)
    - newly attached EBS need to be formatted (except for root EBS)
    - when encrypted at rest the transit to EC2 is enrypted as well
  - Elastic file storage (EFS) - posix filesytem
    - can be accessed by multiple EC2 Instances
    - scales automatically
    - bound to AZ, but accessible in many regions
    - generally cheaper and faster than efs

security groups (firewall rules for EC2, EFS, ELB):

- control how traffic is allowed into or out of EC2 instances
- only contain allow rules (everything else is blocked)
- can reference (source/targets) by IP, security group id or prefix lists
- inbound traffic is blocked by default
- outbound traffic is authorised by default

Distinction between dedicated purchasing options:

- dedicated host: gives you control over hardware to comply to software licenses. Also adds AWS License Manager service.
- dedicated instances: old variant and less control than dedicated host

Spot instances:

- create a Spot Instance Request and if your demand can be met you get an EC2 instance, but it can be killed any time
- you can set a max price you are willing to pay for a Spot Instance Request. But if the average price rises above your max price you will loose all you spot instances.
- with Spot Block you can reserve an spot instance for a time range (1h-6h)
- a Spot Fleet is a set of Spot Instances and optionally On-Demand Instances that is launched based on criteria that you specify.

### 

## service catalog (curated IaC)

AWS Service Catalog lets you centrally manage your cloud resources to achieve governance at scale of your infrastructure as code (IaC) templates, written in CloudFormation or Terraform.

## Aurora Serverless (v2) vs. Aurora XX-Compatible

It is the same service in AWS RDS (relational database service), but a different confiuration. Aurora Serverless automatically scales storage and computing resources. The prices is mostly driven by used ACU (aurora compute units). One ACU is roughly 2GB of used RAM. The minimum amount in IDLE is 0,5 ACU and the scale down takes at least 2 Minutes after scaling up.

If you want to have regional failover instances you can add additional "serverless" instances in other AZs or regions.

[Aurora v1 vs v2](https://aws.amazon.com/de/blogs/aws/amazon-aurora-serverless-v2-is-generally-available-instant-scaling-for-demanding-workloads/)

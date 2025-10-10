<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [AWS Certified Developer - Associate](#aws-certified-developer---associate)
  - [AWS](#aws)
  - [Abbreviations](#abbreviations)
  - [Usecases](#usecases)
  - [Global infrastructure](#global-infrastructure)
    - [Region (e.g. `eu-central-1`)](#region-eg-eu-central-1)
    - [AWS Availability Zones (AZ) (e.g. `eu-central-1a`)](#aws-availability-zones-az-eg-eu-central-1a)
    - [AWS Points of Presence (Edge Locations)](#aws-points-of-presence-edge-locations)
    - [Global Services](#global-services)
    - [Region-scoped](#region-scoped)
  - [IAM: Users & Groups](#iam-users--groups)
    - [IAM: Permissions](#iam-permissions)
    - [IAM Policies structure](#iam-policies-structure)
    - [IAM Roles for Services](#iam-roles-for-services)
    - [Security](#security)
    - [IAM Guidelines and Best Practices](#iam-guidelines-and-best-practices)
    - [Shared Responsibility Model for IAM](#shared-responsibility-model-for-iam)
    - [IAM commands](#iam-commands)
    - [IAM Section – Summary](#iam-section--summary)
  - [How can users access AWS ?](#how-can-users-access-aws-)
    - [AWS CLI & SDK](#aws-cli--sdk)
    - [Create access keys](#create-access-keys)
    - [AWS CloudShell](#aws-cloudshell)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# AWS Certified Developer - Associate

## AWS
* Amazon Web Services
* Provides servers and services that can be used on demand and that scale easily.

## Abbreviations
| Abbreviation | Stands for                                                       |
|--------------|------------------------------------------------------------------|
| ACL          | Access Control Lists                                             |
| ACM          | AWS Certificate Manager                                          |
| ALB          | Application Load Balancer                                        |
| AMI          | Amazon Machine Image                                             |
| arn          | Amazon Resource Name                                             |
| ASG          | Auto-Scaling Group                                               |
| AZ           | Availability Zone                                                |
| CCP          | Certified Cloud Practitioner                                     |
| CIDR         | Classless Inter-Domain Routing (IP address range)                |
| CLB          | Classic Load Balancer                                            |
| CORS         | Cross-Origin Resource Sharing                                    |
| CRR          | Cross-Region Replication                                         |
| CSE          | Client-Side Encryption                                           |
| CSI          | Container Storage Interface                                      |
| DLQ          | Dead Letter Queue                                                |
| DNS Domain   | Name Service                                                     |
| DR           | Disaster Recovery                                                |
| DSSE-KMS     | Dual-layer server-side encryption with AWS Key Management System |
| EBS          | Elastic Block Storage                                            |
| EC2          | Elastic Compute Cloud                                            |
| ECR          | Elastic Container Registry                                       |
| ECS          | Elastic Container Service                                        |
| EFS          | Elastic File System                                              |
| EKS          | Elastic Kubernetes Service                                       |
| ELB          | Elastic Load Balancer                                            |
| ENI          | Elastic Network Interface                                        |
| FIFO         | First In First Out                                               |
| FTP          | File Transfer Protocol                                           |
| FQDN         | Fully Qualified Domain Name                                      |
| GWLB         | Gateway Load Balancer                                            |
| IA           | Infrequent Access                                                |
| IAM          | Identity and Access Management                                   |
| IMDS         | AWS EC2 Instance Metadata                                        |
| IOPS         | Input/Output Operations per Second                               |
| ISP          | Internet Service Provider                                        |
| KMS          | Key Management System/Service                                    |
| KCL          | Kinesis Client Library                                           |
| KPL          | Kinesis Producer Library                                         |
| LRU          | Least Recently Used                                              |
| MSK          | Managed Streaming for Apache Kafka                               |
| NACL         | Network ACL                                                      |
| NLB          | Network Load Balancer                                            |
| NFS          | Network File System                                              |
| NS           | Name Server                                                      |
| OAC          | Origin Access Control                                            |
| RI           | Reserved Instance                                                |
| RDP          | Remote Desktop Protocol                                          |
| RDS          | Relational Database Service                                      |
| SFTP         | Secure File Transfer Protocol                                    |
| SLD          | Second Level Domain                                              | 
| SNI          | Server Name Indication                                           |
| SQS          | Simple Queuing Service                                           |
| SRR          | Same-Region Replication                                          |
| SSE          | Server-Side Encryption                                           |
| SSH          | Secure Shell                                                     |
| TLD          | Top Level Domain                                                 |
| TTL          | Time-to-live                                                     |
| vCPU         | virtual CPU                                                      |
| VPC          | Virtual Private Cloud                                            |
| VPN          | Virtual Private Network                                          |


## Usecases
e.g.
* Enterprise IT
* Backup & Storage
* Big Data analytics
* Website hosting
* Mobile & social apps
* Gaming

## Global infrastructure
* AWS Regions
* AWS Availability Zones
* AWS Data Centers
* AWS Edge Locations / Points of Presence

### Region (e.g. `eu-central-1`)
* Cluster of datacenters
* Compliance with data governance and legal requirements: data never leaves a region without explicit permission
* Proximity to customers: reduced latency
* New services and new features aren't available in every region
* Pricing: pricing varies region to region and is transparent in the service pricing page

### AWS Availability Zones (AZ) (e.g. `eu-central-1a`)
* Each region has many availability zones (usually 3, min is 3, max is 6). Example:  
  - ap-southeast-2a
  - ap-southeast-2b
  - ap-southeast-2c  
* Each availability zone is one or more discrete data centers with redundant power,
networking, and connectivity
* They’re separate from each other, so that they’re isolated from disasters
* They’re connected with high bandwidth, ultra-low latency networking

### AWS Points of Presence (Edge Locations)
* Amazon has 400+ Points of Presence (400+ Edge Locations & 10+ Regional Caches) in 90+ cities across 40+ countries
* Content is delivered to end users with lower latency
* Points of Presence (PoPs): General term used to describe AWS locations where AWS services are accessed. They include both edge locations and regional edge caches.
* Edge Locations: These are specific locations where AWS deploys its content delivery and caching services. They are used primarily by Amazon CloudFront for distributing content and reducing latency for users.
* Regional Edge Caches: These are a subset of edge locations that act as intermediate caches for CloudFront to reduce latency for frequently accessed content.

### Global Services
* Identity and Access Management (IAM)
* Route 53 (DNS service)
* CloudFront (Content Delivery Network)
* WAF (Web Application Firewall)

### Region-scoped
* Amazon EC2 (Infrastructure as a Service)
* Elastic Beanstalk (Platform as a Service)
* Lambda (Function as a Service)
* Rekognition (Software as a Service)
* Region Table: https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services

## IAM: Users & Groups
* IAM = Identity and Access Management, Global service
* Root account created by default, shouldn’t be used or shared
* Users are people within your organization, and can be grouped. 1 person = 1 user.
* Groups (e.g. `admins`, `developers`, ...) only contain users, not other groups.
* Users don’t have to belong to a group (though this is not best practice), and user can belong to multiple groups.

### IAM: Permissions
* Users or Groups can be assigned JSON documents called policies
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "ec2:Describe*",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "elasticloadbalancing:Describe*",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": [
          "cloudwatch:ListMetrics",
          "cloudwatch:GetMetricStatistics",
          "cloudwatch:Describe*"
        ],
        "Resource": "*"
      }
    ]
  }
  ```
* These policies define the permissions of the users
* In AWS you apply the **least privilege principle**: don’t give more permissions than a user needs.
* An IAM user is an identity with long-term credentials that is used to interact with AWS in an account.
* If you attach a policy at the group level --> policy gets applied to every group member.
*  **Inline policy**: Policy only attached to a user

### IAM Policies structure
  ```json
  {
    "Version": "2012-10-17",
    "Id": "S3-Account-Permissions",
    "Statement": [
      {
        "Sid": "1",
        "Effect": "Allow",
        "Principal": {
          "AWS": ["arn:aws:iam::123456789012:root"]
        },
        "Action": [
          "s3:GetObject",
          "s3:PutObject"
        ],
        "Resource": ["arn:aws:s3:::mybucket/*"]
      }
    ]
  }
  ```
Consists of
* Version: policy language version, always include “2012-10-17”
* Id (optional): an identifier for the policy
* Statement: one or more individual statements (required)

Statements consists of
* Sid (statement ID, optional): an identifier for the statement
* Effect: whether the statement allows or denies access (Allow, Deny)
* Principal: account/user/role to which this policy applied to
* Action: list of actions this policy allows or denies (`<service>:<api-call>`, can also be e.g. `ìam:Get*` --> all Get-commands are authorized)
* Resource: list of resources to which the actions apply to. If all resources are allowed --> `"Resource": "*"`
* Condition (optional): conditions for when this policy is in effect


### IAM Roles for Services
* Some AWS service will need to perform actions on your behalf. E.g. Create an EC2 instance (virtual server) --> wants to perform actions on AWS --> EC2 needs permissions
* To do so, we will assign permissions to AWS services with IAM Roles
* Common roles:
  - EC2 Instance Roles
  - Lambda Function Roles
  - Roles for CloudFormation
* IAM --> Roles: `Create role`, Select trusted entity `AWS service` --> choose a service --> add permission --> enter role name and description --> verify trusted entities and permissions --> `Create role`


### Security
* In AWS, you can set up a password policy (in IAM `Account settings`):
  - Set a minimum password length
  - Require specific character types (uppercase, lowercase, numbers, non-alphanumeric characters)
  - Allow all IAM users to change their own passwords
  - Require users to change their password after some time (password expiration)
  - Prevent password re-use
* You can also set up MFA (Google Authenticator, Authy, YubiKey, Hardware Key Fob MFA Device, ...) (click on user in top right --> `Security credentials`)
* IAM Credentials Report (account-level): a report that lists all your account's users and the status of their various credentials (click on `Credential report` in the IAM menu)
* IAM Access Advisor (user-level) (go to `user` and click on `Access Advisor`)
  - Access advisor shows the service permissions granted to a user and when those services were last accessed.
  - You can use this information to revise your policies.


### IAM Guidelines and Best Practices
* One physical user = One AWS user
* Assign users to groups and assign permissions to groups
* Create a strong password policy
* Use and enforce the use of Multi Factor Authentication (MFA)
* Create and use roles for giving permissions to AWS services
* Use Access Keys for Programmatic Access (CLI / SDK)
* Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
* Never share IAM users & Access Keys


### Shared Responsibility Model for IAM
* AWS --> responsible for infrastructure
  - Infrastructure (global network security)
  - Configuration and vulnerability analysis
  - Compliance validation
* I / user --> responsible for how this infrastructure is used
  - Users, Groups, Roles, Policies management and monitoring
  - Enable MFA on all accounts
  - Rotate all your keys often
  - Use IAM tools to apply appropriate permissions
  - Analyze access patterns & review permissions

### IAM commands
* List users: `aws iam list-users`

### IAM Section – Summary
* Users: mapped to a physical user, has a password for AWS Console
* Groups: contains users only
* Policies: JSON document that outlines permissions for users or groups
* Roles: for EC2 instances or AWS services
* Security: MFA + Password Policy
* AWS CLI: manage your AWS services using the command-line
* AWS SDK: manage your AWS services using a programming language
* Access Keys: access AWS using the CLI or SDK
* Audit: IAM Credential Reports & IAM Access Advisor


## How can users access AWS ?
To access AWS, you have three options:
* AWS Management Console (protected by password + MFA) (Best for users who prefer a graphical interface, need to perform manual tasks, or are exploring AWS services.)
* AWS Command Line Interface (CLI): protected by access keys 
* AWS Software Development Kit (SDK) - for code: protected by access keys (Best for developers who need to automate AWS interactions, build applications, or integrate AWS services into their software. It requires programming knowledge and is suitable for more complex, automated, or large-scale tasks.)  
Access Keys are generated through the AWS Console. Users manage their own access keys


### AWS CLI & SDK
* Every command begins with `aws`
* AWS SDK is a set of libraries, enables to access and manage AWS services, and is embedded within my application
* Example: AWS CLI is built on AWS SDK for Python (boto)

### Create access keys
* Click on your user --> `Security credentials` --> `Create access key`
* CLI `aws configure` --> enter access key + default region name + default output format (no necessity to enter anything)

### AWS CloudShell
* CLI in the cloud of AWS
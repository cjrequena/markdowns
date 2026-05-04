---
layout: default
title: aws-naming-conventions
parent: standards
permalink: /docs/standards/aws-naming-conventions
nav_order: 1
---
# AWS Naming Conventions
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

A naming convention is a well-defined set of rules useful for choosing the name of an AWS resource. Ensure that your AWS resources are using appropriate naming conventions for
tagging in order to manage them more efficiently and adhere to AWS resource tagging best practices.

Naming (tagging) your AWS resources consistently has several advantages such as providing additional information about the resource location and usage, promoting consistency within the selected AWS region, distinguishing similar resource stacks from one another, avoiding naming collisions,
improving clarity in cases of potential ambiguity and enhancing aesthetic and professional appearance.

---
## Table of Contents

- [Default Pattern Components](#default-pattern-components)
- [VPC Naming Conventions](#vpc-naming-conventions)
- [Subnet Naming Conventions](#subnet-naming-conventions)
- [Security Group Naming Conventions](#security-group-naming-conventions)
- [Route Tables Naming Conventions](#route-tables-naming-conventions)
- [Internet Gateway Naming Conventions](#internet-gateway-naming-conventions)
- [NAT Gateway Naming Conventions](#nat-gateway-naming-conventions)
- [Network ACL Naming Conventions](#network-acl-naming-conventions)
- [Elastic IP Naming Conventions](#elastic-ip-naming-conventions)
- [EC2 Instance Naming Conventions](#ec2-instance-naming-conventions)
- [Auto Scaling Group Naming Conventions](#auto-scaling-group-naming-conventions)
- [Launch Template Naming Conventions](#launch-template-naming-conventions)
- [PEM Key Naming Conventions](#pem-key-naming-conventions)
- [ALB Naming Conventions](#alb-naming-conventions)
- [NLB Naming Conventions](#nlb-naming-conventions)
- [Target Group Naming Conventions](#target-group-naming-conventions)
- [ECS Cluster Naming Conventions](#ecs-cluster-naming-conventions)
- [ECS Task Definition Naming Conventions](#ecs-task-definition-naming-conventions)
- [ECS Service Naming Conventions](#ecs-service-naming-conventions)
- [EKS Cluster Naming Conventions](#eks-cluster-naming-conventions)
- [ECR Naming Conventions](#ecr-naming-conventions)
- [Lambda Naming Conventions](#lambda-naming-conventions)
- [RDS Naming Conventions](#rds-naming-conventions)
- [DynamoDB Naming Conventions](#dynamodb-naming-conventions)
- [ElastiCache Naming Conventions](#elasticache-naming-conventions)
- [S3 Naming Conventions](#s3-naming-conventions)
- [SQS Naming Conventions](#sqs-naming-conventions)
- [SNS Naming Conventions](#sns-naming-conventions)
- [KMS Naming Conventions](#kms-naming-conventions)
- [Cognito Naming Conventions](#cognito-naming-conventions)
- [UserPool Naming Conventions](#userpool-naming-conventions)
- [IAM Naming Conventions](#iam-naming-conventions)
- [API Gateway Naming Conventions](#api-gateway-naming-conventions)
- [Step Functions Naming Conventions](#step-functions-naming-conventions)
- [CloudWatch Naming Conventions](#cloudwatch-naming-conventions)
- [EFS Naming Conventions](#efs-naming-conventions)
- [Kinesis Naming Conventions](#kinesis-naming-conventions)

---
## Default Pattern Components

**{RegionCode}**     
```(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)```

**{AvailabilityZoneCode}**  
Represents the AZ suffix letter appended to the region (e.g., `a` in `us-east-1a`). AWS uses letters `a`–`f`.  
```([a-f])```

**{EnvironmentCode}**    
```(Dev|Test|Stg|Prod)```

**{AccountCode}**   
Minimum 3 characters. Used in VPC names to distinguish resources across AWS accounts.  
```([A-Z][a-zA-Z0-9]{2,})```

**{ApplicationCode}**     
Minimum 3 characters (one uppercase, one lowercase, one alphanumeric).  
```([A-Z][a-z][a-zA-Z0-9]{1,})```

**{SubnetRouteCode}**   
```(Public|Private)```

**{VersionCode}**   
```(V[0-9]+)```

**{ResourceCode}**   
```(Vpc|Subnet|Sgr|Rt|Igw|Ngw|Nacl|Eip|Ec2|Asg|Lc|Lt|Alb|Nlb|Tg|Ecs|EcsTd|EcsSvc|Eks|Lambda|Rds|Ddb|S3|Sqs|Sns|Kms|Cw|Cf|Agw|Sfn|Iam|Ecr|Efs|Fsx|Elc|Rs|Kin|Glue|Emr|Cognito|UserPool|PemKey)```

---
## VPC Naming Conventions

`{AccountCode}` is included to distinguish VPCs across AWS accounts in multi-account environments.

**Default Pattern Format**

```Vpc-{AccountCode}-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**

```^Vpc-([A-Z][a-zA-Z0-9]{2,})-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**    
<span style="color:#E38D7B;">
Vpc-CorePlatform-UsEast1-Prod-BigDataAppStack
<br>
Vpc-DataTeam-UsWest2-Prod-WebAppStack
</span>

---
## Subnet Naming Conventions

**Default Pattern Format**      
```Subnet-{RegionCode}-{AvailabilityZoneCode}-{SubnetRouteCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Subnet-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-([a-f])-(Public|Private)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**            
<span style="color:#E38D7B;">
Subnet-UsEast1-a-Public-Prod-WebAppStack
<br>
Subnet-UsWest1-b-Private-Prod-DataBaseStack
</span>

---
## Security Group Naming Conventions

**Default Pattern Format**      
```Sgr-{ResourceCode}-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Sgr-(Vpc|Subnet|Sgr|Rt|Igw|Ngw|Nacl|Eip|Ec2|Asg|Lc|Lt|Alb|Nlb|Tg|Ecs|EcsTd|EcsSvc|Eks|Lambda|Rds|Ddb|S3|Sqs|Sns|Kms|Cw|Cf|Agw|Sfn|Iam|Ecr|Efs|Fsx|Elc|Rs|Kin|Glue|Emr|Cognito|UserPool|PemKey)-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Sgr-Ec2-UsWest1-Dev-SampleAppInstance1
</span>

---
## Route Tables Naming Conventions

**Default Pattern Format**      
```Rt-{RegionCode}-{AvailabilityZoneCode}-{SubnetRouteCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Rt-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-([a-f])-(Public|Private)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Rt-UsEast1-a-Public-Prod-WebAppStack
<br>
Rt-EuWest1-b-Private-Dev-ApiService
</span>

---
## Internet Gateway Naming Conventions

**Default Pattern Format**      
```Igw-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Igw-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Igw-UsEast1-Prod-WebAppStack
<br>
Igw-EuWest1-Dev-ApiService
</span>

---
## NAT Gateway Naming Conventions

**Default Pattern Format**      
```Ngw-{RegionCode}-{AvailabilityZoneCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Ngw-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-([a-f])-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Ngw-UsEast1-a-Prod-WebAppStack
<br>
Ngw-EuWest1-b-Dev-ApiService
</span>

---
## Network ACL Naming Conventions

**Default Pattern Format**      
```Nacl-{RegionCode}-{SubnetRouteCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Nacl-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Public|Private)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Nacl-UsEast1-Public-Prod-WebAppStack
<br>
Nacl-EuWest1-Private-Dev-ApiService
</span>

---
## Elastic IP Naming Conventions

**Default Pattern Format**      
```Eip-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Eip-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Eip-UsEast1-Prod-NatGateway
<br>
Eip-EuWest1-Dev-ApiService
</span>

---
## EC2 Instance Naming Conventions

**Default Pattern Format**      
```Ec2-{RegionCode}-{AvailabilityZoneCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Ec2-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-([a-f])-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Ec2-UsEast1-a-Prod-Tomcat
<br>
Ec2-UsWest1-b-Prod-Nodejs
</span>

---
## Auto Scaling Group Naming Conventions

**Default Pattern Format**      
```Asg-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Asg-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Asg-UsEast1-Prod-WebAppStack
<br>
Asg-EuWest1-Dev-ApiService
</span>

---
## Launch Template Naming Conventions

**Default Pattern Format**      
```Lt-{RegionCode}-{EnvironmentCode}-{ApplicationCode}-{VersionCode}```

**RegExp**      
```^Lt-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})-(V[0-9]+)$```

**Examples**        
<span style="color:#E38D7B;">
Lt-UsEast1-Prod-WebAppStack-V1
<br>
Lt-EuWest1-Dev-ApiService-V2
</span>

---
## PEM Key Naming Conventions

**Default Pattern Format**      
```PemKey-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^PemKey-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
PemKey-UsEast1-Prod-WebAppStack
<br>
PemKey-EuWest1-Dev-ApiService
</span>

---
## ALB Naming Conventions

**Default Pattern Format**      
```Alb-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Alb-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Alb-UsEast1-Prod-WebAppStack
<br>
Alb-EuWest1-Dev-ApiService
</span>

---
## NLB Naming Conventions

**Default Pattern Format**      
```Nlb-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Nlb-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Nlb-UsEast1-Prod-InternalService
<br>
Nlb-EuWest1-Dev-DataPipeline
</span>

---
## Target Group Naming Conventions

**Default Pattern Format**      
```Tg-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Tg-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Tg-UsEast1-Prod-WebAppStack
<br>
Tg-EuWest1-Dev-ApiService
</span>

---
## ECS Cluster Naming Conventions

**Default Pattern Format**      
```Ecs-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Ecs-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Ecs-UsEast1-Prod-WebAppStack
<br>
Ecs-EuWest1-Dev-ApiService
</span>

---
## ECS Task Definition Naming Conventions

**Default Pattern Format**      
```EcsTd-{RegionCode}-{EnvironmentCode}-{ApplicationCode}-{VersionCode}```

**RegExp**      
```^EcsTd-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})-(V[0-9]+)$```

**Examples**        
<span style="color:#E38D7B;">
EcsTd-UsEast1-Prod-WebAppStack-V1
<br>
EcsTd-EuWest1-Dev-ApiService-V3
</span>

---
## ECS Service Naming Conventions

**Default Pattern Format**      
```EcsSvc-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^EcsSvc-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
EcsSvc-UsEast1-Prod-WebAppStack
<br>
EcsSvc-EuWest1-Dev-ApiService
</span>

---
## EKS Cluster Naming Conventions

**Default Pattern Format**      
```Eks-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Eks-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Eks-UsEast1-Prod-PlatformCluster
<br>
Eks-EuWest1-Dev-MicroservicesMesh
</span>

---
## ECR Naming Conventions

**Default Pattern Format**      
```Ecr-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Ecr-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Ecr-UsEast1-Prod-WebAppStack
<br>
Ecr-EuWest1-Dev-ApiService
</span>

---
## Lambda Naming Conventions

**Default Pattern Format**      
```Lambda-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Lambda-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Lambda-UsEast1-Prod-WebAppStack
<br>
Lambda-EuWest1-Dev-ApiService
</span>

---
## RDS Naming Conventions

**Default Pattern Format**      
```Rds-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Rds-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Rds-UsEast1-Prod-PatientRecords
<br>
Rds-EuWest1-Dev-UserDatabase
</span>

---
## DynamoDB Naming Conventions

**Default Pattern Format**      
```Ddb-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Ddb-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Ddb-UsEast1-Prod-SessionStore
<br>
Ddb-EuWest1-Dev-EventLog
</span>

---
## ElastiCache Naming Conventions

**Default Pattern Format**      
```Elc-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Elc-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Elc-UsEast1-Prod-SessionCache
<br>
Elc-EuWest1-Dev-ApiCache
</span>

---
## S3 Naming Conventions

**Default Pattern Format**      
```S3-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^S3-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
S3-UsEast1-Prod-ArtifactStore
<br>
S3-EuWest1-Dev-LogArchive
</span>

---
## SQS Naming Conventions

**Default Pattern Format**      
```Sqs-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Sqs-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Sqs-UsEast1-Prod-OrderProcessing
<br>
Sqs-EuWest1-Dev-NotificationQueue
</span>

---
## SNS Naming Conventions

**Default Pattern Format**      
```Sns-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Sns-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Sns-UsEast1-Prod-AlertTopic
<br>
Sns-EuWest1-Dev-EventNotification
</span>

---
## KMS Naming Conventions

**Default Pattern Format**      
```Kms-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Kms-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Kms-UsEast1-Prod-DataEncryption
<br>
Kms-EuWest1-Dev-SecretsKey
</span>

---
## Cognito Naming Conventions

**Default Pattern Format**      
```Cognito-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Cognito-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Cognito-UsEast1-Prod-WebAppStack
<br>
Cognito-EuWest1-Dev-ApiService
</span>

---
## UserPool Naming Conventions

**Default Pattern Format**      
```UserPool-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^UserPool-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
UserPool-UsEast1-Prod-WebAppStack
<br>
UserPool-EuWest1-Dev-ApiService
</span>

---
## IAM Naming Conventions

**Default Pattern Format**      
```Iam-{ResourceCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Iam-(Vpc|Subnet|Sgr|Rt|Igw|Ngw|Nacl|Eip|Ec2|Asg|Lc|Lt|Alb|Nlb|Tg|Ecs|EcsTd|EcsSvc|Eks|Lambda|Rds|Ddb|S3|Sqs|Sns|Kms|Cw|Cf|Agw|Sfn|Iam|Ecr|Efs|Fsx|Elc|Rs|Kin|Glue|Emr|Cognito|UserPool|PemKey)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Iam-Lambda-Prod-DataProcessor
<br>
Iam-Ecs-Dev-ApiService
</span>

---
## API Gateway Naming Conventions

**Default Pattern Format**      
```Agw-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Agw-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Agw-UsEast1-Prod-PublicApi
<br>
Agw-EuWest1-Dev-InternalApi
</span>

---
## Step Functions Naming Conventions

**Default Pattern Format**      
```Sfn-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Sfn-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Sfn-UsEast1-Prod-OrderWorkflow
<br>
Sfn-EuWest1-Dev-DataPipeline
</span>

---
## CloudWatch Naming Conventions

**Default Pattern Format**      
```Cw-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Cw-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Cw-UsEast1-Prod-ApplicationMetrics
<br>
Cw-EuWest1-Dev-InfraAlerts
</span>

---
## EFS Naming Conventions

**Default Pattern Format**      
```Efs-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Efs-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Efs-UsEast1-Prod-SharedStorage
<br>
Efs-EuWest1-Dev-ConfigVolume
</span>

---
## Kinesis Naming Conventions

**Default Pattern Format**      
```Kin-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Kin-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Kin-UsEast1-Prod-EventStream
<br>
Kin-EuWest1-Dev-ClickStream
</span>




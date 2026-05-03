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
## Default Pattern Components

**{RegionCode}**     
```(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)```

**{AvailabilityZoneCode}**  
```([1-2]{1})([a-c]{1})```

**{EnvironmentCode}**    
```(Dev|Test|Stg|Prod)```

**{AccountCode}**   
```([A-Z][a-zA-Z0-9]{2,})```

**{ApplicationCode}**     
```([A-Z][a-z][a-zA-Z0-9]{1,})```

**{SubnetRouteCode}**   
```(Public|Private)```

**{VersionCode}**   
```(V[0-9]+)```

**{ResourceCode}**   
```(Vpc|Subnet|Sgr|Rt|Igw|Ngw|Nacl|Eip|Ec2|Asg|Lc|Lt|Alb|Nlb|Tg|Ecs|Eks|Lambda|Rds|Ddb|S3|Sqs|Sns|Kms|Cw|Cf|Agw|Sfn|Iam|Ecr|Efs|Fsx|Elc|Rs|Kin|Glue|Emr)```

---
## VPC Naming Conventions

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
```^Subnet-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-([1-2]{1})([a-c]{1})-(Public|Private)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**            
<span style="color:#E38D7B;">
Subnet-UsEast1-2a-Public-Prod-WebAppStack
<br>
Subnet-UsWest1-2b-Private-Prod-DataBaseStack
</span>

---
## Security Group Naming Conventions

**Default Pattern Format**      
```Sgr-{ResourceCode}-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Sgr-(Vpc|Subnet|Sgr|Rt|Igw|Ngw|Nacl|Eip|Ec2|Asg|Lc|Lt|Alb|Nlb|Tg|Ecs|Eks|Lambda|Rds|Ddb|S3|Sqs|Sns|Kms|Cw|Cf|Agw|Sfn|Iam|Ecr|Efs|Fsx|Elc|Rs|Kin|Glue|Emr)-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Sgr-Ec2-UsWest1-Dev-SampleAppInstance1
</span>

---
## EC2 Instance Naming Conventions

**Default Pattern Format**      
```Ec2-{RegionCode}-{AvailabilityZoneCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**      
```^Ec2-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-([1-2]{1})([a-c]{1})-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**        
<span style="color:#E38D7B;">
Ec2-UsEast1-2a-Prod-Tomcat
<br>
Ec2-UsWest1-2b-Prod-Nodejs
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


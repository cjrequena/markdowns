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

**{ResourceCode}**   
```(Vpc|Subnet|Sgr|Rt|Igw|Ngw|Nacl|Eip|Ec2|Asg|Lc|Lt|Alb|Nlb|Tg|Ecs|Eks|Lambda|Rds|Ddb|S3|Sqs|Sns|Kms|Cw|Cf|Agw|Sfn|Iam|Ecr|Efs|Fsx|Elc|Rs|Kin|Glue|Emr)```

---
## VPC Naming Conventions

**Default Pattern Format**

```Vpc-{AccountCode}-{RegionCode}-{EnvironmentCode}-{ApplicationCode}```

**RegExp**

```^Vpc-([A-Z][a-zA-Z0-9]{2,})-(UsEast1|UsEast2|UsWest1|UsWest2|CaCentral1|CaWest1|EuWest1|EuWest2|EuWest3|EuCentral1|EuCentral2|EuNorth1|EuSouth1|EuSouth2|ApNortheast1|ApNortheast2|ApNortheast3|ApSoutheast1|ApSoutheast2|ApSoutheast3|ApSoutheast4|ApSouth1|ApSouth2|ApEast1|SaEast1|MeSouth1|MeCentral1|AfSouth1|IlCentral1)-(Dev|Test|Stg|Prod)-([A-Z][a-z][a-zA-Z0-9]{1,})$```

**Examples**

<span style="color:silver;">
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
<span style="color:silver;">
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
<span style="color:silver;">
Sgr-Ec2-UsWest1-Dev-SampleAppInstance1
</span>

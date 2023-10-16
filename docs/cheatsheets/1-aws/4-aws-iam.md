---
layout: default
title:  aws-iam
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-iam
nav_order: 4
---
# AWS IAM
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

- **Identity Access Management** is used to manage access to users and resources.
- IAM is a universal system. (Applied to all regions at the same time). IAM is a free service.
- A root account is the account initially created when AWS is set up (Full administrator).
- New IAM accounts have no permissions by default until granted.
- New users get assigned an Access Key Id an a Secret Key when first created when you give them programmatic access.
- Access Keys are only used for CLI and SDK (Cannot access console).
- Access keys are only shown once when created. If lost they must be deleted and created again.
- Always create a MFA for root account.
- User must enable MFA on their own, administrators cannot turn it on for each user.
- IAM allow you to set password policies to set minimum password requirements or rotate passwords.
- **IAM identities** as Users, Groups and Roles.
- **IAM Users:** End users who log in into the console or interact with AWS resources programmatically.
- **IAM Groups:** Group up your users so they all share permission level of the group eg. Administrators, Developers.
- **IAM Roles:** Associate permissions to a Role and then assign this to a Users or Groups.
- **IAM Policies:** JSON documents which grant permissions for a specific user, group, or role to access services. 
Policies are attached to IAM identities. [Policy structure](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policy-structure.html)
- **Managed Policies:** are policies provided by AWS and cannot be edited.
- **Customer Managed Policies:** are policies created by the customer or user and can be edited.
- **Inline Policies:** are policies which are directly attached to a user. 

**IAM Policy Example**
```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FirstStatement",
      "Effect": "Allow",
      "Action": ["iam:ChangePassword"],
      "Resource": "*"
    },
    {
      "Sid": "SecondStatement",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "ThirdStatement",
      "Effect": "Allow",
      "Action": [
        "s3:List*",
        "s3:Get*"
      ],
      "Resource": [
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ],
      "Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}
    }
  ]
}
```

## Resources
- [Getting Started with AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/getting-started/?nc=sn&loc=3)
- [AWS Identity and Access Management (IAM) Resources](https://aws.amazon.com/iam/resources/?nc=sn&loc=4&iam-blogs.sort-by=item.additionalFields.createdDate&iam-blogs.sort-order=desc)
- [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

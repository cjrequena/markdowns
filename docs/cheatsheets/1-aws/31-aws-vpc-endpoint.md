---
layout: default
title:  aws-vpc-endpoint
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-vpc-endpoint
nav_order: 31
---
# AWS VPC Endpoint CheatSheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

- VPC Endpoints help keep traffic between AWS services whithin the AWS network.
- There are two kind of VPC Endpoints. Interface Endpoints and Gateway Endpoints.
- Interface Endpoints cost money, Gateway Endpoints are free.
- Interface Endpoints uses an Elastic Interface Network (ENI) with private IP (Powered by AWS PrivateLink).
- Gateway Endpoints is a target for a specific route in your route table.
- Interface Endpoints support many AWS services.
- Gateway Endpoints only support DynamoDB and S3

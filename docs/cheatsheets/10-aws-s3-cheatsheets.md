---
layout: default
title: aws-s3-cheatsheet
parent: cheatsheets
nav_order: 10
---
# AWS S3 cheatsheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

- **Simple Storage Service** (S3) Object-based storage. Store **unlimited** amount of data without worry of underlying storage
    infrastructure.
- S3 replicates data across at least 3 AZs to ensure 99.99% Availability and 11'9s of durability.
- Objects contain data (they're like files).
- Objects can be size anywhere from 0 Bytes up to 5 Terabytes.
- Buckets contain objects. Buckets can also contain folders which can in turn can contain objects.
- Bucket names are unique across all AWS accounts. Like a domain name.
- When you upload a file to S3 successfully you'll receive a HTTP 200 code.
- **Lifecycle management** Objects can be moved between storage classes or objects be deleted automatically based on schedule.
- **Versioning** Objects are giving a version ID. Whe new objects are uploaded the old objects are kept. You can access any
    version. When you delete an object the previous object is restored. Once the version is turned on it cannot be turn off,
    only suspended.
- **MFA delete** Enforce DELETE operations to require a MFA token in order to delete an object. Must have versioning turn on 
    to use. Can only turn on MFA Delete from AWS CLI. Root account is only allowed to delete objects.
- All new buckets are private by default.
- Logging can be turned to on a bucket to log to track operations performed on objects.
- **Access control** is configured using **Bucket Policies** and **Access Control List**
- **Bucket Policies** are JSON documents which let you write complex control access.
- **ACLs** are legacy method (not deprecated) where you grant access to objects and buckets with simple actions.
- **Security in Transit** Uploading files is done over SSL
- **SSE** stand for Server Side Encryption. AWS has 3 options for SSE.
- **SSE-AES** S3 handles the key, uses AES-256 algorithm.
- **SSE-KMS** Envelope encryption via AWS KMS and you manage the keys.
- **SSE-C** Customer provided keys (you manage the keys).
- **Client-Side Encryption** You must encrypt your own files before uploading them to S3.
- **Cross Region Replication (CRR)** allows you to replicate files across regions for greater durability. You must have versioning
    turned on in the source and the destination bucket. You can have CRR to replicate to a bucket in another AWS account.
- **Transfer Acceleration** provide faster and secure uploads from anywhere in the world. Data is uploaded via distinct url
    to an Edge Location. Data is transported to your S3 bucket via AWS backbone network.
- **Presigned URLs** is a URL generated via the AWS CLI and SDK. It provides temporary access to write or download object data.
    Presigned URLs are commonly used to access private objects.

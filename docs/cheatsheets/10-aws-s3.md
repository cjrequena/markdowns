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
- Objects consist of the following:  
  Key: This is simply the name of the object.  
  Value: This is simply the data and is made up of a sequence of Bytes.  
  VersionId: Used for versioning  
  Metadata: Data about the data you are storing  
  Sub-resources: ACLs, torrent
- Buckets contain objects. Buckets can also contain folders which can in turn can contain objects.
- Bucket names are unique across all AWS accounts. Like a domain name.
- When you upload a file to S3 successfully you'll receive a HTTP 200 code.
- S3 data consistency:  
  Read after write consistency for PUTS of new objects    
  Eventual consistency for overwrite PUTS and DELETES
- S3 performance:
  3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per partitioned prefix  
  There are no limits to the number of prefixes in a bucket
- **S3 storage classes and tiers:**  
  **S3 Standard:** 99.99% availability and 99.99999999999% durability, store your objects redundantly across multiple devices in multiple facilities and is designed to sustain the loss of two facilities concurrently.   
  **S3-IA Infrequent Access:** Is for data that is less frequently accessed but requires rapid access when needed. It has a lower fee than S3 Standard, but you will be charged a retrieval fee.  
  **S3 One Zone Infrequent Access:** For where you want a low-cost option for infrequently accessed data, but that does not require the multiple availability zone data resilience.  
  **S3 Intelligent-Tiering:** Designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead.  
  **S3 Glacier:** Is a secure, durable and low-cost storage class for data archiving. You can reliably store any amount of data at costs that are competitive with or cheaper than on-premise solutions. Retrieval times is configurable from minutes to hours.  
  **S3 Glacier Deep Archiving:** Is the amazon S3 lower-cost storage class where a retrieval time of 12 hours is acceptable.
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

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

- **Simple Storage Service** (S3) Object-based storage. Store unlimited amount of data without worry of underlying storage
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

---
layout: default
title: aws-efs
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-efs
nav_order: 7
---
# template
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## What is Amazon EFS?

- Amazon Elastic File System (EFS) is a managed file storage service that can be shared across multiple Amazon EC2 instances.
- It provides scalable, highly available, and durable network-attached storage (NAS) for AWS workloads.

## Key Concepts

1. **File System**: The top-level directory structure that you create within an EFS file system.

2. **Mount Target**: A network interface used to mount an EFS file system on an EC2 instance in a specific Availability Zone (AZ).

3. **Security Group**: A security group acts as a virtual firewall for your EFS mount targets.

4. **Lifecycle Management**: EFS supports automatic data lifecycle management, which includes transitioning files to the Infrequent Access storage class after a specified period.

5. **Throughput Modes**:
    - **Bursting Throughput Mode**: Suitable for workloads with varying access patterns.
    - **Provisioned Throughput Mode**: Suitable for applications with predictable workloads.

## EFS Storage Classes

Amazon EFS offers two storage classes with different performance and cost characteristics:

1. **Standard Storage Class**:
    - Suitable for frequently accessed files.
    - Designed for low-latency access.
    - Ideal for general-purpose workloads.
    - Supports bursting throughput.

2. **One Zone Storage Class**:
    - Data is stored within a single Availability Zone (AZ).
    - Cost-effective for infrequently accessed data and backup scenarios.
    - Lower-cost option compared to Standard EFS.
    - May not provide the same level of availability as Standard EFS.

## Quotas

Amazon EFS has certain quotas and limits that you should be aware of. These limits can vary by AWS Region, and you can request a limit increase if needed:

1. **File System Size**:
    - Standard EFS: 8 TB to 8 PB per file system.
    - One Zone EFS: 1 TB to 8 PB per file system.

2. **File System Throughput**:
    - Standard EFS: Up to 100 MiB/s per TiB of storage.
    - One Zone EFS: Up to 100 MiB/s per TiB of storage.

3. **Number of Mount Targets**: Default limit is 1,000 per Region.

4. **Number of NFSv4.1 Named Users and Groups**: Default limit is 1,000 per Region.

5. **Per-User and Per-Group ID Mapping**: Default limit is 500 per user or group per file system.

6. **Number of Network Interfaces for Mount Targets**: Default limit is 1,000 per Region.

7. **File System Lifecycle Policies**: Up to 10 per file system.

8. **File System Tags**: Up to 50 tags per file system.

## Amazon EFS Performance

Performance considerations for EFS:
- Latency: EFS has low-latency access for read and write operations.
- Throughput: You can scale throughput based on the size of the file system.
- IOPS: EFS doesn't have a concept of IOPS.

## Data Sync and Migration

- You can use the AWS DataSync service or rsync for data synchronization and migration to and from EFS.

This cheat sheet provides a basic overview of Amazon EFS, its storage classes, and key quotas. Be sure to check the AWS documentation for the latest details and any region-specific limits.

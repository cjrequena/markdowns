---
layout: default
title: aws-efs
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-efs
nav_order: 7
---
# Amazon Elastic File System (EFS) Cheat Sheet
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

## Limits and Constrains

Amazon Elastic File System (EFS) has various constraints and limits that you should be aware of when designing your file 
storage solutions. These constraints may change over time, so it's important to check the AWS documentation for the 
most up-to-date information. As of my last knowledge update in January 2022, here are some of the key constraints 
and limits for Amazon EFS:

1. **File System Size**:
    - Standard EFS: 8 terabytes (TB) to 8 petabytes (PB) per file system.
    - One Zone EFS: 1 TB to 8 PB per file system.

2. **File System Throughput**:
    - Standard EFS: Throughput scales linearly with the size of the file system, with a maximum of 100 MiB/s per TiB of storage.
    - One Zone EFS: Throughput also scales linearly with a maximum of 100 MiB/s per TiB of storage.

3. **Number of Mount Targets**: The default limit is 1,000 mount targets per Region.

4. **Number of NFSv4.1 Named Users and Groups**: The default limit is 1,000 named users and groups per Region.

5. **Per-User and Per-Group ID Mapping**: The default limit is 500 per user or group per file system.

6. **Number of Network Interfaces for Mount Targets**: The default limit is 1,000 network interfaces for mount targets per Region.

7. **File System Lifecycle Policies**: You can configure up to 10 lifecycle policies per file system.

8. **File System Tags**: You can attach up to 50 tags to a file system.

9. **Data Consistency**: EFS provides strong read-after-write consistency, but it may take some time for changes to propagate across all AZs.

10. **Security Group Rules**: Security groups for EFS mount targets can have rules that allow or deny network traffic. Ensure that your security group rules are properly configured.

11. **Directory and File Size**: The maximum directory and file size supported is 2 TiB.

12. **Access Control**: EFS uses POSIX-compliant access control lists (ACLs) to manage access to files and directories.

13. **Network Bandwidth**: EFS does not limit network bandwidth for data transfer to and from the file system.

14. **Throughput Modes**: EFS supports Bursting Throughput and Provisioned Throughput modes, each with specific performance characteristics and limits.

15. **Lifecycle Management**: EFS supports automatic data lifecycle management, including transitioning files to the Infrequent Access storage class after a specified period.

16. **One Zone Storage Class**: One Zone EFS stores data in a single Availability Zone, which may have reduced availability compared to Standard EFS.

It's important to note that these constraints and limits may vary by AWS Region, and AWS may allow you to request limit increases for certain resources. Always consult the AWS documentation and AWS support for the most current and region-specific information regarding Amazon EFS constraints and limits.

This cheat sheet provides a basic overview of Amazon EFS, its storage classes, and key quotas. Be sure to check the AWS documentation for the latest details and any region-specific limits.

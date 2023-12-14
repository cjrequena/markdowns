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

Amazon Elastic Block Store (EBS) and Amazon Elastic File System (EFS) are both storage solutions provided by AWS, but they serve different purposes and have distinct characteristics. Here's a comparison between EBS and EFS:

## EFS vs EBS

### Use Cases

1. **EBS (Elastic Block Store):**
   - Suitable for applications that require low-latency and high-performance block-level storage.
   - Ideal for use as boot volumes for EC2 instances, databases, and applications that require dedicated storage.

2. **EFS (Elastic File System):**
   - Suitable for shared file storage scenarios where multiple EC2 instances need access to the same file system.
   - Ideal for content management systems, web serving environments, and applications with shared data requirements.

### Storage Type:

1. **EBS:**
   - Provides block-level storage that is directly attached to EC2 instances.
   - EBS volumes are used as raw block devices, and file systems need to be created on top of them.

2. **EFS:**
   - Provides file-level storage accessible by multiple EC2 instances concurrently.
   - Offers a file system interface, and data can be accessed concurrently from multiple EC2 instances using NFS protocols.

### Accessibility:

1. **EBS:**
   - Attached to a single EC2 instance at a time.
   - Suitable for scenarios where data needs to be localized to a specific instance.

2. **EFS:**
   - Can be mounted on multiple EC2 instances simultaneously.
   - Ideal for scenarios where multiple instances need shared access to the same data.

### Performance:

1. **EBS:**
   - Performance is tied to the specific EBS volume type chosen (e.g., General Purpose, Provisioned IOPS, Throughput Optimized, Cold HDD).
   - Suited for applications with specific I/O and throughput requirements.

2. **EFS:**
   - Performance scales automatically with the size of the file system.
   - Designed for general-purpose workloads with moderate throughput requirements.

### Scaling:

1. **EBS:**
   - Scaling involves resizing or adding additional EBS volumes to individual EC2 instances.
   - Each EBS volume is attached to a specific EC2 instance.

2. **EFS:**
   - Scales horizontally with the number of EC2 instances that can mount the file system.
   - No need to manually resize; it grows and shrinks as files are added or removed.

### Durability and Availability:

1. **EBS:**
   - EBS volumes are replicated within an Availability Zone (AZ) for durability.
   - Supports snapshots for backup and recovery.

2. **EFS:**
   - Data is automatically replicated across multiple Availability Zones (AZs) within a region for high availability.
   - Suitable for use cases requiring higher availability and redundancy.

### Cost Model:

1. **EBS:**
   - Billed based on provisioned capacity (per GB) and the chosen volume type.
   - Cost is incurred for each EBS volume attached to an EC2 instance.

2. **EFS:**
   - Billed based on the amount of data stored (per GB).
   - Additional costs may apply for data transfer and requests.

### Summary:

- Use EBS for applications that require dedicated block-level storage and need to be localized to specific EC2 instances.
- Use EFS for shared file storage scenarios where multiple EC2 instances need concurrent access to the same file system.

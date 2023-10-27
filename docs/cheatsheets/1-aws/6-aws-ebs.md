---
layout: default
title: aws-ebs
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-ebs
nav_order: 6
---
# AWS EBS
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## EBS
EBS is a block storage service that provides scalable, high-performance storage volumes for use with Amazon EC2 instances. 
Below, you'll find information on the various EBS storage classes and their respective quotas as of my last knowledge update 
in January 2022. Please note that AWS service offerings and quotas may have changed since then, so it's essential to check the 
AWS documentation for the most up-to-date information. (Amazon Elastic Block Store endpoints and quotas)[https://docs.aws.amazon.com/general/latest/gr/ebs-service.html]

## Amazon EBS Storage Classes

### General Purpose SSD (gp2):

- **Volume Size**: 1 GiB to 16 TiB

- **Throughput**: 128 MiB/s per GiB of volume size, with a minimum of 100 MiB/s

- **IOPS**: 3 IOPS per GiB of volume size, with a minimum of 100 IOPS and a maximum of 16,000 IOPS

### Provisioned IOPS SSD (io2 and io1):

Amazon Elastic Block Store (EBS) io classes, specifically io1 and io2, are designed for applications that require high levels of input/output operations per second (IOPS) and
low-latency storage performance. Below is a detailed overview of the EBS io classes as of my last knowledge update in January 2022. Please verify the most up-to-date information in
the AWS documentation, as offerings may have evolved since then.

- **Volume Size**: 4 GiB to 16 TiB

- **Throughput**: 256 KiB/s per provisioned IOPS, with a minimum of 100 MiB/s

- **IOPS**: Up to 64,000 IOPS (io2) or 32,000 IOPS (io1) per volume

- **Max IOPS-to-Volume Size Ratio**: 500 IOPS per GiB (io2) or 50 IOPS per GiB (io1)

#### **io1 (Provisioned IOPS SSD):**

- **Use Cases:** io1 volumes are suitable for applications with high I/O requirements, such as relational databases (e.g., MySQL, PostgreSQL), NoSQL databases (e.g., Cassandra), and transactional applications.

- **Volume Size:** Ranging from 4 GiB to 16 TiB.

- **IOPS Performance:** You can provision a specific number of IOPS per volume, ranging from 100 IOPS to a maximum of 32,000 IOPS.

- **Throughput:** The throughput of io1 volumes is directly related to the provisioned IOPS, with 256 KiB/s per provisioned IOPS as the baseline.

- **Durability:** io1 volumes are designed to provide high durability and availability for critical applications.

#### **io2 (Provisioned IOPS SSD):**

- **Use Cases:** io2 volumes are designed for critical production workloads that require consistently high IOPS performance, low-latency storage, and enhanced durability. They are suitable for applications like SAP HANA, high-performance databases, and financial applications.

- **Volume Size:** Ranging from 4 GiB to 16 TiB.

- **IOPS Performance:** io2 volumes support provisioned IOPS ranging from 100 IOPS to a maximum of 64,000 IOPS. They also offer Burst IOPS, which allows you to burst to 64,000 IOPS for volumes at or above 100 GiB in size.

- **Throughput:** The throughput is based on the provisioned IOPS, with 256 KiB/s per provisioned IOPS as the minimum throughput.

- **Durability:** io2 volumes are designed to deliver the highest level of data durability compared to other EBS volume types. They provide 99.999% durability.

- **Burst Capability:** For io2 volumes 100 GiB and larger, there is a burst capability that allows the volume to burst to 64,000 IOPS and 1,000 MiB/s for up to 30 minutes.

- **Multi-Attach Support:** io2 volumes support Multi-Attach, which enables attaching a single EBS volume to multiple EC2 instances simultaneously. This is useful for applications that require shared storage access.

- **Provisioned IOPS to Volume Size Ratio:** With io2 volumes, you can achieve higher IOPS per GiB, with a maximum ratio of 4,000 IOPS per GiB.

- **IO2 Block Express:** This is a sub-category of io2 volumes designed for ultra-high performance and lower-latency workloads. IO2 Block Express volumes offer high IOPS and throughput with a provisioned IOPS-to-volume-size ratio of 4,000 IOPS per GiB.

Remember that when choosing between io1 and io2 volumes, io2 is often preferred due to its enhanced durability, burst capability, and consistent 
high performance, making it suitable for a wide range of demanding applications. Additionally, io2 Block Express provides the highest levels of IOPS and throughput performance. Always consult the latest 
AWS documentation for any updates or changes to EBS offerings and performance characteristics.

### Throughput Optimized HDD (st1):

- *Volume Size*: 500 GiB to 16 TiB

- *Throughput*: A baseline of 40 MiB/s per TiB of volume size, with a burst rate of up to 250 MiB/s per TiB

### Cold HDD (sc1):

- *Volume Size*: 500 GiB to 16 TiB

- *Throughput*: A baseline of 12 MiB/s per TiB of volume size, with a burst rate of up to 80 MiB/s per TiB

- **Magnetic (standard):**
    
    - *Volume Size*: 1 GiB to 1 TiB
  
    - *Throughput*: About 40-90 MiB/s

- **Provisioned IOPS SSD (io2 Block Express):**

    - *Volume Size*: 4 GiB to 64 TiB
  
    - *Throughput*: 256 KiB/s per provisioned IOPS, with a minimum of 100 MiB/s
  
    - *IOPS*: Up to 256,000 IOPS per volume
  
    - *Max IOPS-to-Volume Size Ratio*: 4,000 IOPS per GiB

- **NVMe-based SSD (gp3):**

    - *Volume Size*: 1 GiB to 16 TiB
  
    - *Throughput*: 125 MiB/s per TiB of volume size, with a minimum of 1,000 MiB/s for volumes under 5.2 TiB
  
    - *IOPS*: 3,000 IOPS per TiB of volume size

- **NVMe-based SSD (io2 Block Express):**
    
    - *Volume Size*: 4 GiB to 64 TiB
  
    - *Throughput*: 256 KiB/s per provisioned IOPS, with a minimum of 1,000 MiB/s
  
    - *IOPS*: Up to 128,000 IOPS per volume
  
    - *Max IOPS-to-Volume Size Ratio*: 4,000 IOPS per GiB

**Additional Information:**

- Snapshots of EBS volumes are available for backup, replication, and disaster recovery.

- Encryption can be enabled on EBS volumes.

- You can attach and detach EBS volumes from EC2 instances.

- EBS volumes are specific to an Availability Zone.

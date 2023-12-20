---
layout: default
title: aws-rds
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-rds
nav_order: 10
---
# AWS RDS 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Introduction to AWS RDS:

AWS RDS is a managed relational database service that simplifies database administration tasks, such as hardware provisioning, 
database setup, patching, and backups. It supports various database engines, including MySQL, PostgreSQL, Oracle, SQL Server, 
and MariaDB.

- RDS stands for Relational Database Service.
- It is a managed DB service and use SQL as query language.
- It allows you to create databases in the cloud that are managed by AWS
  - Postgres
  - MySQL
  - MariaDB
  - Oracle
  - Microsoft SQL Server
  - Aurora (AWS Proprietary Database)

## Advantages

- RDS is a managed service.
- Automated provisioning, OS Patching.
- Continuous backups and restore to a specific point in time (Point in time restore).
- Monitoring dashboards.
- Read replicas for improved read performance.
- Multi AZ setup for Disaster Recovery and high availability.
- Maintenance windows for upgrades.
- Scaling capabilities (Vertical and Horizontal)
- Storage backed by EBS (gp2 or io1)
- You cannot ssh into your instances.  

## Key Concepts:

1. **DB Instances:**
    - Basic building blocks of RDS.
    - Represent a single database deployed in the cloud.

2. **Multi-AZ Deployments:**
    - Provides high availability by replicating the database in a secondary Availability Zone.

3. **Read Replicas:**
    - Used to scale read-heavy database workloads.
    - Asynchronous replication from the primary DB instance.

4. **Parameter Groups:**
    - Configure database engine settings.
    - Applied to DB instances at launch or during modification.

5. **Option Groups:**
    - Enable features and functionalities for DB instances.
    - Applied during DB instance creation or modification.

6. **Snapshots:**
    - Point-in-time backups of DB instances.
    - Used for database restoration.

## Security:

1. **VPCs and Security Groups:**
    - RDS instances reside in VPCs and are associated with security groups.
    - Security groups control inbound and outbound traffic.

2. **IAM Authentication:**
    - Use IAM roles to manage database access.
    - Authenticate using AWS Identity and Access Management.

3. **Encryption:**
    - RDS supports encryption at rest and in transit.
    - Use AWS Key Management Service (KMS) for encryption key management.

## Monitoring and Management:

1. **CloudWatch Metrics:**
    - Monitor RDS performance using CloudWatch.
    - Metrics include CPU utilization, storage, and database connections.

2. **Enhanced Monitoring:**
    - Provides additional monitoring insights.
    - Granular view of database performance.

3. **Event Notifications:**
    - Set up notifications for database events.
    - Use Amazon SNS for alerting.

## Maintenance:

1. **Automated Backups:**
    - Daily automated backups with a retention period.
    - Enables point-in-time recovery.

2. **DB Snapshots:**
    - Manually initiated snapshots for backup and recovery.

3. **Maintenance Windows:**
    - Define a window for routine maintenance.
    - Automated backups and software patching occur during this window.

## RDS Storage Auto Scaling

- Helps you increase storage on your RDS DB instance dynamically.
- When RDS detects you are running out of free database storage, it scales automatically.
- Avoid manually scaling your database storage.
- You have to set a **Maximum Storage Threshold (Maximum Limit)
- Automatically modify storage if:
    - Free storate is less than 10% of allocated storage.
    - Low-storage lasts at least 5 minutes.
    - 6 hours have passed since last modification.
- Useful for applications with unpredictable workloads.

## RDS Read Replicas
- Up to 15 read replicas within AZ, cross AZ, cross region.
- Replication is ASYNC so reads are eventually consistent.
- Replicas can be promoted to their own DB.
- Applications must update the connection string to leverage read replicas,

## RDS Multi AZ
- Mainly used for disaster recovery.
- SYNC replication.
- One DNS name -- automatic app fail over to standby.
- Increase the availability.
- Fail over in case of loss of AZ, loss of network, instance or storage failure.
- No manual intervention in apps.
- Not used for scaling.

## RDS Custom
- Full admin access to the underlying OS and the database.
- For Managed Oracle and Microsoft SQL Server.

## RDS Backups
- Automated backups:
    - Daily full backup of the database (during the backup window).
    - Transaction logs are backed-up by RDS every 5 minutes.
    - Ability to restore to any point in time(from oldest backup to 5 minutes ago).
    - 1 to 35 days of retention, set 0 to disable automated backups.
- Manual backups:
    - Manually triggered by the user
    - Retention of backup for as long as you want.
- Trick: In a stopped RDS database, you will pay for storage. If you plan on stopping it for a long period of time, make a snapshot and restore instead.

## Amazon Aurora
- Aurora is a propietary technology from AWS (Not open source).
- Postgres and MySQL are both supported by Aurora DB (that means your driver will work as if Aurora was a Postgres or MySQL database)
- Aurora is "AWS Cloud Optimized" and claims 5x performance improvement over MySQL and 3x over Postgres on RDS.
- Aurora storage automatically grows in increments of 10 GB, up to 128 TB.
- Aurora can have up to 15 read replicas and the replication process is faster than MySQL.
- Failover in Aurora is instantaneous. It is HA native.
- Aurora costs more the RDS (20% more).

### Aurora HA and Read Scaling
- 6 copies of your data across 3 AZ
- One Aurora instance takes writes (master)
- Automated Failover for master in less than 30 seconds.
- Master + up to 15 read replicas.
- Support cross region replication

### Aurora DB Cluster
- Writer Endpoint pointing to the master.
- Reader Endpoint pointing to a load balancer that poing to an autoscaling instances.


## Aurora Backups
- Automated backups:
  - 1 to 35 days of retention (cannot be disabled).
  - Point in time recovery in that timeframe.
- Manual backups:
    - Manually triggered by the user
    - Retention of backup for as long as you want.

## RDS and Aurora Security

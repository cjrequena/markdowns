---
layout: default
title:  aws-ec2
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-ec2
nav_order: 5
---
# AWS EC2
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## EC2

- Elastic Cloud Computing (EC2) is a Cloud Computing Service.
- Configure your EC2 by choosing your OS, Storage, Memory, Networking Throughput.
- Launch and SSH into your serve within minutes.
- EC2 comes in a variety Instance Types specialized for different roles:
  - **General purpose** balance of compute, memory and networking resources.
  - **Compute Optimized** ideal for compute bound applications that benefit from high performance processor.
  - **Memory Optimized** fast performance for workloads that process large data sets in memory.
  - **Accelerated Optimized** high, sequential read and write access to very large data sets on local storage.
- Instance Size generally double in price and key feature.
- **Placement Groups** let you choose the logical placement of your instances to optimize communication, performance or durability. Placement Groups are free.
- **User Data** a script that will be automatically run when launching an EC2 instance.
- **MetaData** meta data about the current instance. You access this meta data via local end-point when SSH into the EC2 instance. eg ```sh curl http://169.254.169.254/latest/meta-data```
- **Instance Profile** a container for an IAM role that you can use to pass role information to an EC2 instance when the instance starts.

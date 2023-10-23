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

## EC2 Basics

1. **EC2 Instance Types:**
- EC2 instances come in various types optimized for different workloads (e.g., t2.micro, m5.large, c5.xlarge).

2. **Regions and Availability Zones:**
- Choose the AWS region and availability zone carefully for fault tolerance and proximity to users.

3. **Amazon Machine Images (AMIs):**
- AMIs are pre-configured templates for creating EC2 instances. Choose an AMI that fits your requirements.

4. **Security Groups:**
- Security groups act as virtual firewalls for EC2 instances. Define inbound/outbound rules to control network traffic.

5. **Key Pairs:**
- Use key pairs to securely access EC2 instances. Create and store the private key securely.

## Launching and Managing EC2 Instances

6. **EC2 Launch Wizard:**
- AWS provides an EC2 Launch Wizard to simplify instance setup for specific use cases.

7. **Launch Templates:**
- Use launch templates to standardize instance configurations and simplify instance launches.

8. **Auto Scaling:**
- Set up auto scaling groups to automatically adjust the number of instances based on load or a schedule.

9. **Elastic Load Balancing:**
- Use ELB to distribute incoming traffic across multiple EC2 instances for high availability and fault tolerance.

10. **Instance Store vs. EBS:**
- Understand the differences between instance store and Elastic Block Store (EBS) for storage options.

11. **Instance Metadata:**
- EC2 instances can access metadata for configuration details via http://169.254.169.254/latest/meta-data/.

## Elastic Block Store (EBS)

12. **EBS Volume Types:**
- EBS offers various volume types such as GP2, IO1, SC1, ST1, and gp3 with different performance characteristics.

13. **Snapshots:**
- Create EBS snapshots for data backups and recovery. Snapshots are incremental and stored in Amazon S3.

14. **EBS Encryption:**
- You can encrypt EBS volumes at rest for enhanced security.

15. **EBS Performance:**
- Optimize EBS performance with appropriate volume type, size, and IOPS (input/output operations per second).

## Networking

16. **VPC (Virtual Private Cloud):**
- Isolate your network resources using VPC. Define subnets, route tables, and security groups.

17. **Elastic IP Addresses:**
- Use Elastic IPs for static, public IPv4 addresses. Associate them with EC2 instances to ensure they retain the same IP even if instances are replaced.

18. **VPC Peering:**
- Connect VPCs to enable communication between instances in different VPCs.

19. **Network Interfaces:**
- Attach additional network interfaces to EC2 instances for multi-networking.

20. **Elastic Network Adapter (ENA):**
- ENA is a high-performance network interface for enhanced networking capabilities.

## Monitoring and Management

21. **CloudWatch:**
- Use CloudWatch for monitoring EC2 instances, setting up alarms, and collecting performance data.

22. **Instance Metadata and User Data:**
- EC2 instances can retrieve instance metadata and user data for custom configuration during instance startup.

23. **Tagging:**
- Apply tags to EC2 instances for organization and resource management.

24. **EC2 Instance Roles:**
- Assign IAM roles to EC2 instances to grant them permissions to access AWS services.

25. **EC2 Systems Manager:**
- Use Systems Manager for patch management, automation, and maintenance of EC2 instances.

## EC2 Pricing Model

### On Demand

- Least commitment
- Only pay per hour
- Short term, spiky, unpredictable workloads
- Cannot be interrupted
- For first time apps

### Reserved

- Best long-term
- Up to 75% off
- Steady state or predictable usage
- Commit to EC2 over a 1 or 3 years term
- Can sell unused reserved instances (Reserved Instances Marketplace)
- Can be shared between multiple account within an organization
- **Standard** Up to 75% reduced price, cannot change the RI attributes
- **Convertible** Up to 54% reduced pricing, allows you to change RI attributes if greater or equal in value
- **Scheduled** You reserve instances for specific time periods. Eg. once a week for a few hours. Savings vary
- **Terms** You commit to a 1 year or 3 years contract. The longer the term the greater the savings.
- **Payment Options** All Upfront, Partial Upfront, and No Upfront. The greater the upfront the greater the savings.

### Spot

- Biggest savings
- Up to 90% off
- Request spare computing capacity
- Flexible start and end times
- Can handle interruptions (Sever randomly starting and stopping)
- For non-critical background jobs

### Dedicated

- Most expensive
- Dedicated servers
- Can be on-demand or reserved
- When you need guarantee or isolate hardware (Enterprise requirements)

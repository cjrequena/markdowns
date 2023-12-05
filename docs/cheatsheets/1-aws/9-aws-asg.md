---
layout: default
title: aws-asg
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-asg
nav_order: 9
---
# AWS Auto Scaling Group (ASG) Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## AWS Auto Scaling Group (ASG)
An Auto Scaling Group (ASG) in AWS allows you to automatically scale your EC2 instances based on conditions you define. 

The goal of an Auto Scaling group is to:

- Scale out (Add EC2 instances) to match an increased load.
- Scale in (Remove EC2 instances) to match a decreased load.
- Ensure we have a minimum and a maximum number of EC2 instances running.
- Automatically register new instances to a load balancer.
- Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy).

The Auto Scaling Group is free.

### Basic Concepts:
- **Auto Scaling Group (ASG):** Logical grouping of EC2 instances that automatically adjusts its size based on policies.
- **Launch Configuration:** (Deprecated) Defines settings for EC2 instances on how to launch them within an  ASG:
  - AMI
  - Instance Type
  - EC2 User Data
  - EBS Volumes
  - Security Groups
  - SSH Key Pair
  - IAM Role for your EC2 Instances
  - Network + Subnets information
  - Load Balancer information
- **Launch Template:** Similar to a launch configuration, but allows versioning and parameterization.
- **Min Size / Max Size / Initial Capacity**
- **Scaling Policies**
  - It is possible to scale and ASG based on CloudWatch Alarms
  - An alarms monitor a metric (Such as Average CPU, or custom metric)
  - Metrics such as Average CPU are computed for the overall ASG instances
  - Base on the alarm;
    - We can create scale-out policies (increase the number of instances)
    - We can create scale-in policies (decrease the number of instances)

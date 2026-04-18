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

The goal of an Auto Scaling Group is to:

- **Scale out** (add EC2 instances) to match an increased load.
- **Scale in** (remove EC2 instances) to match a decreased load.
- Ensure a **minimum** and **maximum** number of EC2 instances are running.
- Automatically register new instances to a load balancer.
- Re-create an EC2 instance if a previous one is terminated (e.g., if unhealthy).

> ASG itself is free — you only pay for the underlying EC2 instances and resources launched.

## Basic Concepts

- **Auto Scaling Group (ASG)**: Logical grouping of EC2 instances that automatically adjusts its size based on policies.
- **Launch Template** (recommended): Defines how to launch EC2 instances within an ASG. Supports versioning, parameterization, and mixed instance types. Includes:
  - AMI + Instance Type
  - EC2 User Data
  - EBS Volumes
  - Security Groups
  - SSH Key Pair
  - IAM Role for EC2 Instances
  - Network + Subnets information
  - Load Balancer / Target Group information
  - Spot/On-Demand configuration
- **Launch Configuration** (deprecated): Similar to Launch Template but does not support versioning, mixed instances, or Spot configuration. Migrate to Launch Templates.

### ASG Capacity Settings

| Setting | Description |
|---|---|
| **Minimum capacity** | Minimum number of instances the ASG will maintain. |
| **Desired capacity** | The number of instances the ASG tries to maintain. Adjusted by scaling policies. |
| **Maximum capacity** | Maximum number of instances the ASG can scale to. |

## Health Checks

ASG uses health checks to determine if instances are healthy. Unhealthy instances are terminated and replaced.

| Health Check Type | Description |
|---|---|
| **EC2** (default) | Instance is unhealthy if the EC2 status check fails (hardware/software issues). |
| **ELB** | Instance is unhealthy if the ELB health check fails (application-level check). Recommended when using a load balancer. |
| **Custom** | Use with VPC Lattice or custom health check endpoints. |

- **Health check grace period**: Time (default: 300 seconds) after instance launch before health checks begin. Allows time for the instance to bootstrap.
- If an instance fails health checks, ASG terminates it and launches a replacement.

## Scaling Policies

Scaling policies adjust ASG capacity based on CloudWatch Alarms. Alarms monitor metrics (e.g., Average CPU) computed across all ASG instances.

### Simple Scaling

- Adds or removes a **fixed number** of instances when a CloudWatch alarm is triggered.
- Waits for the cooldown period before evaluating again.
- **Note**: AWS recommends using Step Scaling instead of Simple Scaling, as Step Scaling can respond to additional alarms during cooldown.
- Use case: Basic workloads with predictable, steady-state demand.

### Step Scaling

- Adjusts capacity in **multiple steps** based on the size of the alarm breach.
- Each step defines a different adjustment value for a different threshold range.
- Does **not** wait for cooldown — can respond to multiple alarms simultaneously.
- Use case: Workloads with variable demand that need proportional responses.

### Target Tracking Scaling

- Adjusts capacity to maintain a **target value** for a specific metric.
- Example: Keep average CPU utilization at 50%.
- ASG automatically creates and manages the CloudWatch alarms.
- Use case: Most common and simplest policy for dynamic workloads.

### Scheduled Scaling

- Adjusts capacity at **specific times** based on a schedule (cron or one-time).
- Define start/end times and desired capacity for each scheduled action.
- Use case: Known traffic patterns (e.g., scale up before business hours).

### Predictive Scaling

- Uses **machine learning** to analyze historical load patterns and forecast future traffic.
- Proactively scales capacity **ahead of predicted demand**.
- Works alongside other scaling policies.
- Use case: Cyclical or recurring traffic patterns where reactive scaling is too slow.

## Scaling Cooldowns

- After a scaling activity, ASG enters a **cooldown period** (default: 300 seconds).
- During cooldown, ASG does not launch or terminate additional instances.
- Prevents rapid, oscillating scale-out/scale-in cycles.
- **Tip**: Use a ready-to-use AMI to reduce instance configuration time, which allows you to reduce the cooldown period.
- Step Scaling and Target Tracking policies can override the default cooldown with their own warm-up time.

## Lifecycle Hooks

Lifecycle hooks let you perform custom actions as ASG launches or terminates instances.

- **Launching hook**: Instance enters `Pending:Wait` state before going `InService`. Use to install software, pull configuration, register with external services.
- **Terminating hook**: Instance enters `Terminating:Wait` state before being terminated. Use to collect logs, deregister from services, drain connections.
- Default timeout: **3600 seconds** (1 hour). Configurable up to 48 hours (with heartbeat).
- Integrates with **EventBridge**, **SNS**, **SQS**, or **Lambda** for automation.

## Termination Policies

When ASG needs to scale in, it uses termination policies to decide which instance to terminate.

| Policy | Description |
|---|---|
| **Default** | 1) Select AZ with most instances. 2) Terminate instance with oldest launch template/configuration. 3) Terminate instance closest to next billing hour. |
| **OldestInstance** | Terminate the oldest instance in the group. |
| **NewestInstance** | Terminate the newest instance in the group. |
| **OldestLaunchConfiguration** | Terminate instances using the oldest launch configuration. Useful during rolling updates. |
| **OldestLaunchTemplate** | Terminate instances using the oldest launch template version. |
| **ClosestToNextInstanceHour** | Terminate the instance closest to the next billing hour. |
| **AllocationStrategy** | Align with the allocation strategy for Spot/On-Demand mixed instances. |

- You can combine multiple policies (evaluated in order).
- **Instance Protection**: You can protect specific instances from scale-in termination.
- **Instance Scale-In Protection** can be set at the ASG level or on individual instances.

## Instance Standby

You can put an instance into **Standby** state to troubleshoot or perform maintenance without ASG terminating it.

- Instance is removed from the load balancer (deregistered from target group).
- ASG does **not** replace the instance while it is in standby.
- You can choose whether ASG decrements the desired capacity or not.
- Return the instance to `InService` when maintenance is complete.
- Use case: Debugging a specific instance, applying patches, or investigating issues without affecting the ASG.

## Instance Refresh

Instance Refresh allows you to update all instances in an ASG in a rolling fashion (e.g., after AMI update).

- Set a **minimum healthy percentage** (e.g., 90%) — ASG replaces instances while keeping at least that percentage healthy.
- Optionally set a **warm-up time** for new instances before they are considered healthy.
- Supports **checkpoints** to pause the refresh at a certain percentage for validation.
- Triggers: Launch template change, new AMI, configuration update.

## Warm Pools

Warm Pools maintain a pool of **pre-initialized instances** that can be quickly placed into service.

- Instances in the warm pool are in a `Stopped`, `Running`, or `Hibernated` state.
- When ASG scales out, it pulls from the warm pool instead of launching cold instances.
- Significantly reduces scale-out latency (seconds vs minutes).
- You pay for stopped instances (EBS volumes) but not for compute.
- Use case: Applications with long bootstrap times.

## Mixed Instances Policy

Allows an ASG to use a combination of **On-Demand** and **Spot Instances** across multiple instance types.

- Define a **base capacity** of On-Demand instances.
- Define the **percentage split** above base (e.g., 70% Spot, 30% On-Demand).
- Specify multiple instance types for diversification (reduces Spot interruption risk).
- ASG automatically selects the optimal instance type based on availability and price.
- Use case: Cost optimization while maintaining availability.

## Suspend and Resume Processes

You can suspend specific ASG processes to temporarily disable certain behaviors.

| Process | Description |
|---|---|
| `Launch` | Suspends launching new instances. |
| `Terminate` | Suspends terminating instances. |
| `HealthCheck` | Suspends health check evaluations. |
| `ReplaceUnhealthy` | Suspends replacement of unhealthy instances. |
| `AZRebalance` | Suspends rebalancing instances across AZs. |
| `AlarmNotification` | Suspends acting on CloudWatch alarm notifications. |
| `ScheduledActions` | Suspends scheduled scaling actions. |
| `AddToLoadBalancer` | Suspends registering new instances with the load balancer. |
| `InstanceRefresh` | Suspends instance refresh operations. |

- Use case: Suspend `Terminate` and `ReplaceUnhealthy` during debugging to prevent ASG from terminating instances you're investigating.
- Use case: Suspend `AZRebalance` during maintenance in a specific AZ.
- **Warning**: Suspending `Launch` or `Terminate` for extended periods can cause the ASG to drift from its desired state.

## Activity History and Notifications

- ASG records all scaling activities in the **Activity History** (visible in the console or via API).
- Each activity entry includes: cause, status, start/end time, and affected instances.
- Configure **SNS notifications** for the following events:
  - Instance launch
  - Instance terminate
  - Instance launch error
  - Instance terminate error
- Integrate with **Amazon EventBridge** for more advanced event-driven automation (e.g., trigger Lambda on scale-out).

## CloudWatch Metrics for Auto Scaling

### ASG-Level Metrics

| Metric | Description |
|---|---|
| `GroupDesiredCapacity` | Desired capacity of the ASG. |
| `GroupInServiceInstances` | Number of running instances that passed health checks. |
| `GroupPendingInstances` | Number of instances pending launch. |
| `GroupStandbyInstances` | Number of instances in standby state. |
| `GroupTerminatingInstances` | Number of instances being terminated. |
| `GroupInServiceCapacity` | Total capacity units in InService state. |
| `GroupTotalCapacity` | Total capacity units in the group. |
| `GroupMaxSize` | Maximum size of the ASG. |
| `GroupMinSize` | Minimum size of the ASG. |

### Common Scaling Metrics

| Metric | Description |
|---|---|
| `CPUUtilization` | Average CPU utilization across all ASG instances. |
| `NetworkIn` / `NetworkOut` | Network traffic in/out of ASG instances. |
| `RequestCountPerTarget` | Average request count per target (from ALB). |
| Custom metrics | Any custom CloudWatch metric pushed from your application. |

## Integration with ELB

- ASG automatically registers new instances with the attached **target group(s)**.
- ASG automatically deregisters terminated instances.
- When using **ELB health checks**, ASG can terminate instances that fail application-level checks.
- Supports ALB, NLB, and CLB integration.

### Quick Setup: ASG with ALB

1. Create an ALB with target group and health checks.
2. Create a Launch Template (AMI, instance type, security groups, user data).
3. Create an ASG → attach the Launch Template → set min/desired/max capacity.
4. Attach the ALB target group to the ASG.
5. Configure scaling policies (Target Tracking recommended).
6. Monitor via CloudWatch → adjust policies based on traffic patterns.

## Quotas and Constraints

| Quota | Default Value |
|---|---|
| Max ASGs per region | 200 |
| Max launch configurations per region | 200 |
| Max scaling policies per ASG | 50 |
| Max scheduled actions per ASG | 125 |
| Max lifecycle hooks per ASG | 50 |
| Max step adjustments per step scaling policy | 20 |
| Max instances per ASG | Determined by desired capacity and account EC2 limits |
| Max warm pool size per ASG | Equal to the difference between max capacity and desired capacity |

> **Note**: Quotas can be increased via AWS Service Quotas. Check the [ASG Quotas documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-quotas.html) for latest values.

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

## Basic Concepts:
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

## Scaling Policies
  - It is possible to scale and ASG based on CloudWatch Alarms
  - An alarms monitor a metric (Such as Average CPU, or custom metric)
  - Metrics such as Average CPU are computed for the overall ASG instances
  - Base on the alarm;
    - We can create scale-out policies (increase the number of instances)
    - We can create scale-in policies (decrease the number of instances)

## Creating an Auto Scaling Group (ASG) with an Application Load Balancer (ALB) using the AWS Management Console

### Step 1: Create an Application Load Balancer (ALB)

1. **Navigate to the EC2 Dashboard:**
    - Open the AWS Management Console and go to the EC2 dashboard.

2. **Create Load Balancer:**
    - Click on "Load Balancers" in the left navigation pane.
    - Click the "Create Load Balancer" button.

3. **Choose Load Balancer Type:**
    - Select "Application Load Balancer" and click "Create."

4. **Configure Load Balancer:**
    - Give your ALB a name.
    - Configure listeners (usually HTTP on port 80 or HTTPS on port 443).
    - Configure security settings and optionally configure the routing.

5. **Configure Security Groups:**
    - Create a new security group or use an existing one.
    - Configure inbound rules to allow traffic on the required ports.

6. **Configure Routing:**
    - Set up target groups for routing traffic to instances.
    - Review and create the ALB.

### Step 2: Create Launch Configuration

1. **Navigate to the EC2 Dashboard:**
    - In the AWS Management Console, go to the EC2 dashboard.

2. **Create Launch Configuration:**
    - Click on "Launch Configurations" in the left navigation pane.
    - Click the "Create launch configuration" button.

3. **Choose AMI and Instance Type:**
    - Select an Amazon Machine Image (AMI) for your instances.
    - Choose an instance type.
    - Configure details like user data, IAM role, etc.

4. **Configure Auto Scaling:**
    - In the "Advanced Details" section, configure user data or other advanced settings.
    - Configure storage, security groups, and key pairs.

5. **Review and Create:**
    - Review your configuration settings and click "Create launch configuration."

### Step 3: Create Auto Scaling Group (ASG)

1. **Navigate to the EC2 Dashboard:**
    - In the AWS Management Console, go to the EC2 dashboard.

2. **Create Auto Scaling Group:**
    - Click on "Auto Scaling Groups" in the left navigation pane.
    - Click the "Create Auto Scaling group" button.

3. **Choose Launch Configuration:**
    - Select the launch configuration you created earlier.

4. **Configure Auto Scaling Group Details:**
    - Give your Auto Scaling group a name.
    - Set the desired capacity, minimum, and maximum size.
    - Configure subnets for your instances.

5. **Configure Scaling Policies:**
    - Choose "Use scaling policies to adjust the capacity of this group."
    - Add scaling policies or create them later.

6. **Configure Notifications:**
    - Set up notifications if desired.

7. **Configure Tags:**
    - Add any necessary tags.

8. **Configure Advanced Details:**
    - Configure health checks, placement groups, etc.

9. **Review and Create:**
    - Review your configuration settings and click "Create Auto Scaling group."

### Step 4: Attach Auto Scaling Group to ALB

1. **Navigate to the EC2 Dashboard:**
    - In the AWS Management Console, go to the EC2 dashboard.

2. **Select Auto Scaling Group:**
    - Click on "Auto Scaling Groups" in the left navigation pane.
    - Select the Auto Scaling group you created.

3. **Edit Auto Scaling Group:**
    - Click the "Edit" button.

4. **Configure Load Balancer:**
    - In the "Load balancing" section, select your ALB.
    - Configure target group(s).

5. **Review and Confirm:**
    - Review your changes and click "Update Auto Scaling group."

### Step 5: Test Auto Scaling

1. **Monitor ASG:**
    - Go to the Auto Scaling group dashboard.
    - Monitor instances and scaling activities.

2. **Test Load Balancer:**
    - Access your ALB's DNS name.
    - Observe traffic being distributed among instances.


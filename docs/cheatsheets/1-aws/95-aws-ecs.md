---
layout: default
title: aws-ecs
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-ecs
nav_order: 95
---
# AWS Elastic Container Service (ECS) Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## AWS Elastic Container Service (ECS)

Amazon ECS is a fully managed container orchestration service that allows you to run, stop, and manage Docker containers on a cluster.

- Supports two launch types: **EC2** and **Fargate**.
- Integrates natively with ALB, NLB, CloudWatch, IAM, Secrets Manager, and other AWS services.
- Supports Docker containers and OCI-compatible images.

## Core Concepts

| Concept | Description |
|---|---|
| **Cluster** | Logical grouping of tasks and services. Can span multiple AZs. |
| **Task Definition** | Blueprint for your application (like a Dockerfile for ECS). Defines containers, images, CPU, memory, ports, volumes, environment variables. |
| **Task** | A running instance of a Task Definition. Can contain one or more containers. |
| **Service** | Maintains a desired number of tasks running and integrates with load balancers. Handles task replacement if a task fails. |
| **Container Instance** | An EC2 instance registered to an ECS cluster (EC2 launch type only). |

## Launch Types

### EC2 Launch Type

- You provision and manage the EC2 instances (container instances) in the cluster.
- You must run the **ECS Agent** on each EC2 instance (included in ECS-optimized AMIs).
- You are responsible for patching, scaling, and managing the underlying infrastructure.
- You pay for the EC2 instances regardless of container utilization.
- Use case: Need full control over infrastructure, GPU workloads, specific instance types, or cost optimization with Reserved Instances/Savings Plans.

### Fargate Launch Type

- **Serverless** — no EC2 instances to manage.
- AWS manages the underlying infrastructure.
- You define CPU and memory at the task level.
- Each task gets its own ENI (Elastic Network Interface) → own private IP.
- You pay only for the vCPU and memory your tasks consume.
- Use case: Simplicity, no infrastructure management, variable workloads, microservices.

### EC2 vs Fargate Comparison

| Feature | EC2 | Fargate |
|---|---|---|
| Infrastructure management | You manage | AWS manages |
| Pricing | Per EC2 instance | Per task vCPU + memory |
| Scaling infrastructure | You scale (ASG) | Automatic |
| SSH access to host | Yes | No |
| GPU support | Yes | No |
| Persistent storage | EBS, EFS | EFS only |
| Placement constraints | Full control | Limited |
| Startup time | Faster (if capacity available) | Slightly slower (cold start) |

## Task Definitions

A task definition is a JSON document that describes one or more containers (up to 10) that form your application.

### Key Parameters

| Parameter | Description |
|---|---|
| `family` | Name of the task definition family (groups revisions). |
| `containerDefinitions` | Array of container configurations. |
| `cpu` / `memory` | Task-level CPU and memory (required for Fargate). |
| `networkMode` | `awsvpc` (required for Fargate), `bridge`, `host`, or `none`. |
| `taskRoleArn` | IAM role the task assumes to call AWS APIs. |
| `executionRoleArn` | IAM role ECS uses to pull images and push logs. |
| `volumes` | Data volumes for containers. |
| `requiresCompatibilities` | `EC2`, `FARGATE`, or both. |
| `runtimePlatform` | OS family and CPU architecture (e.g., Linux/ARM64). |

### Container Definition Key Fields

| Field | Description |
|---|---|
| `name` | Container name. |
| `image` | Docker image (e.g., `123456789.dkr.ecr.us-east-1.amazonaws.com/my-app:latest`). |
| `cpu` / `memory` / `memoryReservation` | Container-level resource allocation. |
| `portMappings` | Host-to-container port mappings. |
| `environment` | Environment variables (key-value pairs). |
| `secrets` | Secrets from SSM Parameter Store or Secrets Manager. |
| `logConfiguration` | Log driver configuration (e.g., `awslogs`). |
| `healthCheck` | Container health check command. |
| `essential` | If `true`, task stops if this container stops. At least one container must be essential. |
| `dependsOn` | Container startup/shutdown dependencies. |

### Network Modes

| Mode | Description |
|---|---|
| `awsvpc` | Each task gets its own ENI and private IP. Required for Fargate. Recommended for EC2. |
| `bridge` | Uses Docker's built-in virtual network (default for EC2). Dynamic port mapping with ALB. |
| `host` | Bypasses Docker networking; container uses host's network directly. No port mapping. |
| `none` | No external network connectivity. |

## IAM Roles for ECS

| Role | Purpose |
|---|---|
| **EC2 Instance Role** | Allows the ECS Agent on EC2 instances to communicate with ECS, pull images from ECR, push logs to CloudWatch. Attached to the container instance. |
| **Task Execution Role** | Allows ECS to pull container images from ECR, fetch secrets from SSM/Secrets Manager, and push logs. Used by the ECS service itself. |
| **Task Role** | Allows containers within the task to call AWS APIs (e.g., S3, DynamoDB). Each task definition can have a different role (least privilege). |

> **Important**: Task Role ≠ Task Execution Role. Task Role is for your application code. Task Execution Role is for ECS infrastructure operations.

## ECS Services

A service ensures a specified number of task instances are running and can integrate with load balancers.

### Service Configuration

| Setting | Description |
|---|---|
| **Desired count** | Number of task instances to maintain. |
| **Launch type** | EC2 or Fargate. |
| **Deployment configuration** | Min/max healthy percent during deployments. |
| **Load balancer** | ALB or NLB target group association. |
| **Service discovery** | AWS Cloud Map integration for DNS-based discovery. |
| **Platform version** | Fargate platform version (e.g., `1.4.0`, `LATEST`). |

### Deployment Strategies

| Strategy | Description |
|---|---|
| **Rolling update** (default) | Replaces tasks gradually. Controlled by `minimumHealthyPercent` and `maximumPercent`. |
| **Blue/Green** (CodeDeploy) | Deploys new tasks alongside old ones, then shifts traffic. Supports canary and linear shifts. Requires ALB. |
| **External** | Use a third-party deployment controller. |

### Rolling Update Parameters

- `minimumHealthyPercent` (default: 100%): Minimum percentage of desired tasks that must remain running during deployment.
- `maximumPercent` (default: 200%): Maximum percentage of desired tasks allowed during deployment (includes new + old tasks).
- Example: desired=4, min=50%, max=200% → can terminate 2 old tasks and launch up to 4 new tasks simultaneously.

## Load Balancer Integration

### Application Load Balancer (ALB)

- Recommended for most use cases.
- Supports **dynamic port mapping** (EC2 bridge mode) — multiple tasks on the same instance.
- Supports path-based and host-based routing.
- With `awsvpc` mode, ALB routes directly to task ENIs.

### Network Load Balancer (NLB)

- Use for high throughput, low latency, or TCP/UDP workloads.
- Required for AWS PrivateLink integration.
- Supports static IPs and elastic IPs.

> **Note**: Classic Load Balancer is supported but not recommended (no dynamic port mapping, no Fargate support).

## Auto Scaling

ECS Service Auto Scaling uses **Application Auto Scaling** to adjust the desired count of tasks.

### Scaling Metrics

| Metric | Description |
|---|---|
| `ECSServiceAverageCPUUtilization` | Average CPU utilization across all tasks in the service. |
| `ECSServiceAverageMemoryUtilization` | Average memory utilization across all tasks. |
| `ALBRequestCountPerTarget` | Average request count per target in the ALB target group. |
| Custom metrics | Any CloudWatch metric via step or target tracking policies. |

### Scaling Policy Types

| Type | Description |
|---|---|
| **Target Tracking** | Maintain a target value for a metric (e.g., CPU at 70%). Simplest and most common. |
| **Step Scaling** | Scale in steps based on CloudWatch alarm breach size. |
| **Scheduled Scaling** | Scale at specific times (e.g., scale up before peak hours). |

### Scaling the Infrastructure (EC2 Launch Type)

- Use **ECS Cluster Capacity Providers** to automatically scale EC2 instances.
- Capacity Provider is linked to an ASG.
- **Managed scaling**: ECS automatically adjusts the ASG desired capacity based on task demand.
- **Target capacity %**: Percentage of instances that should be running tasks (e.g., 80% keeps buffer capacity).

## Capacity Providers

Capacity Providers manage the infrastructure for your tasks.

| Provider | Description |
|---|---|
| `FARGATE` | Uses Fargate for tasks. |
| `FARGATE_SPOT` | Uses Fargate Spot (up to 70% discount, with 2-minute interruption notice). |
| Custom (ASG-based) | Links to an ASG for EC2 launch type with managed scaling. |

- **Capacity Provider Strategy**: Define a mix of providers with `base` and `weight`.
  - `base`: Minimum number of tasks on this provider (only one provider can have base > 0).
  - `weight`: Relative proportion of tasks placed on this provider.
- Example: `FARGATE` base=1, weight=1 + `FARGATE_SPOT` weight=3 → 1 task always on Fargate, then 75% on Spot, 25% on Fargate.

## Task Placement (EC2 Launch Type Only)

When launching tasks on EC2, ECS determines where to place tasks using placement strategies and constraints.

### Placement Strategies

| Strategy | Description |
|---|---|
| `binpack` | Place tasks on instances with the least available CPU or memory. Minimizes instance count (cost optimization). |
| `random` | Place tasks randomly across instances. |
| `spread` | Spread tasks evenly across a specified field (e.g., `attribute:ecs.availability-zone`, `instanceId`). |

- Strategies can be **combined** (evaluated in order).
- Example: Spread by AZ, then binpack by memory.

### Placement Constraints

| Constraint | Description |
|---|---|
| `distinctInstance` | Each task is placed on a different container instance. |
| `memberOf` | Place tasks on instances matching a Cluster Query Language expression (e.g., `attribute:ecs.instance-type =~ t3.*`). |

## Data Volumes and Storage

### Bind Mounts

- Share data between containers in the same task.
- Ephemeral — data is lost when the task stops.
- Works on both EC2 and Fargate.
- Fargate ephemeral storage: 20 GiB default, configurable up to 200 GiB.

### EFS (Elastic File System)

- Persistent, shared storage across tasks and AZs.
- Works with both EC2 and Fargate launch types.
- Mount EFS file systems into containers.
- Use case: Shared data, CMS content, ML models.
- Supports EFS Access Points for fine-grained access control.

### EBS (EC2 Launch Type Only)

- Attach EBS volumes to container instances.
- Not natively supported at the task level (use Docker volumes plugin).

## Logging and Monitoring

### CloudWatch Logs (awslogs driver)

- Most common logging solution for ECS.
- Configure in task definition under `logConfiguration`.
- Requires the **Task Execution Role** to have `logs:CreateLogStream` and `logs:PutLogEvents` permissions.

```json
"logConfiguration": {
  "logDriver": "awslogs",
  "options": {
    "awslogs-group": "/ecs/my-app",
    "awslogs-region": "us-east-1",
    "awslogs-stream-prefix": "ecs"
  }
}
```

### Other Log Drivers

| Driver | Description |
|---|---|
| `splunk` | Send logs to Splunk. |
| `fluentd` | Send logs to Fluentd. |
| `awsfirelens` | Route logs to multiple destinations using Fluent Bit or Fluentd sidecar. Supports S3, Kinesis, OpenSearch, etc. |

### CloudWatch Container Insights

- Provides detailed metrics at the cluster, service, and task level.
- Metrics: CPU, memory, network, storage, running task count.
- Enables dashboards and alarms for container workloads.

## Service Discovery (AWS Cloud Map)

- Automatically registers/deregisters tasks in AWS Cloud Map.
- Provides DNS-based or API-based service discovery.
- Tasks can find each other using DNS names (e.g., `my-service.my-namespace.local`).
- Supports health checks to remove unhealthy instances from DNS.
- Use case: Microservices communication without a load balancer.

## Secrets Management

| Source | How to Reference |
|---|---|
| **SSM Parameter Store** | `valueFrom: arn:aws:ssm:region:account:parameter/name` |
| **Secrets Manager** | `valueFrom: arn:aws:secretsmanager:region:account:secret:name` |

- Secrets are injected as environment variables or mounted as files.
- Requires the **Task Execution Role** to have permissions to read the secrets.
- Secrets are fetched at task launch time.

## ECS Anywhere

- Run ECS tasks on **on-premises** or customer-managed infrastructure.
- Install the ECS Agent and SSM Agent on external instances.
- Register external instances to your ECS cluster.
- No support for Fargate, ALB, or service discovery on external instances.
- Use case: Hybrid deployments, edge computing, data sovereignty.

## ECS Exec

- Run commands interactively inside a running container (like `docker exec`).
- Uses SSM Session Manager under the hood.
- Requires `enableExecuteCommand` on the service/task and appropriate IAM permissions.

```bash
aws ecs execute-command \
  --cluster my-cluster \
  --task <task-id> \
  --container my-container \
  --interactive \
  --command "/bin/sh"
```

## Common CLI Commands

```bash
# Cluster operations
aws ecs create-cluster --cluster-name my-cluster
aws ecs list-clusters
aws ecs describe-clusters --clusters my-cluster
aws ecs delete-cluster --cluster my-cluster

# Task definitions
aws ecs register-task-definition --cli-input-json file://task-def.json
aws ecs list-task-definitions --family-prefix my-app
aws ecs describe-task-definition --task-definition my-app:1
aws ecs deregister-task-definition --task-definition my-app:1

# Tasks
aws ecs run-task --cluster my-cluster --task-definition my-app:1 --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-xxx],securityGroups=[sg-xxx],assignPublicIp=ENABLED}"
aws ecs list-tasks --cluster my-cluster --service-name my-service
aws ecs describe-tasks --cluster my-cluster --tasks <task-id>
aws ecs stop-task --cluster my-cluster --task <task-id>

# Services
aws ecs create-service --cluster my-cluster --service-name my-service \
  --task-definition my-app:1 --desired-count 2 --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-xxx],securityGroups=[sg-xxx],assignPublicIp=ENABLED}"
aws ecs list-services --cluster my-cluster
aws ecs describe-services --cluster my-cluster --services my-service
aws ecs update-service --cluster my-cluster --service my-service --desired-count 4
aws ecs delete-service --cluster my-cluster --service my-service --force

# ECS Exec
aws ecs execute-command --cluster my-cluster --task <task-id> --container my-container \
  --interactive --command "/bin/sh"

# Container instances (EC2 launch type)
aws ecs list-container-instances --cluster my-cluster
aws ecs deregister-container-instance --cluster my-cluster --container-instance <instance-id> --force
```

## Security Best Practices

- Use **Task Roles** with least privilege (one role per task definition).
- Use **awsvpc** network mode for task-level security groups.
- Store secrets in **Secrets Manager** or **SSM Parameter Store** — never in environment variables as plain text.
- Use **private subnets** with NAT Gateway or VPC endpoints for ECR, CloudWatch, and Secrets Manager.
- Enable **CloudTrail** for ECS API auditing.
- Use **ECR image scanning** to detect vulnerabilities in container images.
- Enable **encryption at rest** for EFS volumes and secrets.
- Restrict container privileges: avoid `privileged` mode, use `readonlyRootFilesystem`, drop unnecessary Linux capabilities.

## Pricing

| Launch Type | You Pay For |
|---|---|
| **EC2** | EC2 instances (regardless of container usage), EBS volumes, data transfer. |
| **Fargate** | vCPU and memory per second (from task start to stop), ephemeral storage above 20 GiB. |
| **Fargate Spot** | Same as Fargate but up to 70% discount. Tasks can be interrupted with 2-minute notice. |

- ECS itself is **free** — you only pay for the underlying compute and resources.
- Additional costs: ALB/NLB, CloudWatch Logs, data transfer, EFS, ECR storage.

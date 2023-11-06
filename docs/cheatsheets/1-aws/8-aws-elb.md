---
layout: default
title: aws-elb
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-elb
nav_order: 8
---
# AWS Elastic Load Balancer (ELB) Cheat Sheet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## AWS Elastic Load Balancer (ELB)

A Load Balancer is a networking device or service that distributes incoming network traffic (such as web requests or application data) 
across multiple servers or instances to ensure high availability, reliability, and optimal performance. The primary purpose of a load balancer 
is to evenly distribute the incoming requests and traffic to the backend servers, preventing any single server from being overwhelmed and improving 
the overall efficiency of the system. Load balancers are commonly used in web applications, websites, and other distributed systems to achieve fault
tolerance, scalability, and redundancy.

**AWS Elastic Load Balancer (ELB)**, often referred to simply as AWS ELB, is Amazon Web Services' fully managed load balancing service. ELB is 
designed to automatically distribute incoming application traffic across multiple Amazon Elastic Compute Cloud (EC2) instances, containers, and 
other resources within an AWS Virtual Private Cloud (VPC). AWS ELB is highly available, scalable, and integrates seamlessly with other AWS services, 
making it a popular choice for load balancing in cloud-based applications.

There are four classes of AWS ELB:

1. **Application Load Balancer (ALB)**: Designed for distributing HTTP and HTTPS traffic, ALB offers advanced routing capabilities and content-based routing, making it suitable for web applications and microservices.

2. **Network Load Balancer (NLB)**: Optimized for handling TCP and UDP traffic, NLB provides high-performance, low-latency load balancing with support for static IP addresses, and it preserves the source IP address of the client. It is an excellent choice for applications that require high performance and low latency.

3. **Classic Load Balancer (deprecated)**: The Classic Load Balancer is a legacy option supporting both HTTP/HTTPS and TCP/UDP traffic. It provides basic load balancing capabilities and is recommended for existing users to migrate to ALB or NLB for enhanced features and performance.

4. **Gateway Load Balancer**: Gateway Load Balancer helps you easily deploy, scale, and manage your third-party virtual appliances. It gives you one gateway for distributing traffic across multiple virtual appliances while scaling them up or down, based on demand. This decreases potential points of failure in your network and increases availability.

AWS ELB automatically routes traffic to healthy instances, monitors the health of the backend resources, and scales based on demand. It also offers 
features like SSL termination, sticky sessions, security group integration, and access logs. ELB plays a crucial role in ensuring the availability 
and reliability of applications hosted on AWS by distributing traffic across multiple instances or resources.

## ELB Classes:

1. **Application Load Balancer (ALB)**:
   - Designed for HTTP/HTTPS traffic and provides advanced routing, content-based routing, and support for containers.
   - Supports routing based on host, path, query parameters, and more.
   - Best suited for web applications and microservices.

2. **Network Load Balancer (NLB)**:
   - Ideal for TCP/UDP traffic and provides high-performance, low-latency load balancing.
   - Supports static IP addresses and preserves the source IP of the client.
   - Best for applications requiring extreme performance.

3. **Classic Load Balancer** (Deprecated - Consider migrating to ALB or NLB):
   - Supports both HTTP and TCP/UDP traffic.
   - Provides basic load balancing capabilities.
   - Legacy load balancer class, consider migrating to ALB or NLB for enhanced features.
   
4. **Gateway Load Balancer**:
   - xxx
   - xxx
   - xxx

### Quotas and Constraints:

- **Application Load Balancer (ALB)**:
  - Max Listeners per Load Balancer: 75
  - Max Target Groups per Load Balancer: 75
  - Max Rules per Listener: 50
  - Max URLs per Rule: 100
  - Idle Connection Timeout: 60 seconds

- **Network Load Balancer (NLB)**:
  - Max Listeners per Load Balancer: 25
  - Max Target Groups per Load Balancer: 25
  - Idle Connection Timeout: Configurable (Default: 350 seconds)
  - Max Elastic IP addresses: Per your AWS account limits

- **Classic Load Balancer** (Deprecated):
  - Max Listeners per Load Balancer: 1
  - Max Instances per Load Balancer: 100
  - Idle Connection Timeout: 60 seconds

- **Gateway Load Balancer**
  - xxx
  - xxx
  - xxx


## AWS ELB Rules Cheat Sheet

### 1. Listener Rules:

- **Description**: Listener rules define how incoming traffic is routed to target groups. They are specific to Application Load Balancers (ALB).

- **Key Elements**:
  - **Priority**: Numeric order of rule evaluation (lower numbers are evaluated first).
  - **Conditions**: Define conditions for the rule to match (e.g., path, host, query string).
  - **Actions**: Specify the target group to route matched traffic to.

- **Example**:
  - Priority: 1
  - Condition: If the path is '/api', route to Target Group A.
  - Priority: 2
  - Condition: If the host is 'api.example.com', route to Target Group B.

### 2. Target Group Rules:

- **Description**: Target group rules determine how traffic within a target group is routed to registered instances. Specific to ALB.

- **Key Elements**:
  - **Path Patterns**: Define URL path patterns to route to different instances.
  - **Path Patterns Conditions**: Match specific paths and route accordingly.
  - **Weighted Routing**: Assign weights to instances for proportional traffic distribution.

- **Example**:
    - Path Pattern: /images/* -> Route to instances in Target Group A.
    - Path Pattern: /videos/* -> Route to instances in Target Group B.
    - Weighted Routing: 75% to Target Group C, 25% to Target Group D.

### 3. Host-Based Routing:

- **Description**: Specific to ALB, allows routing based on the host header in HTTP requests.

- **Use Cases**: Route traffic to different applications or microservices based on the domain name in the URL.

- **Example**:
  - Route traffic for example.com to Target Group A.
  - Route traffic for api.example.com to Target Group B.

### 4. Path-Based Routing:

- **Description**: Specific to ALB, allows routing based on the URL path.

- **Use Cases**: Route traffic to different paths or endpoints within your application.

- **Example**:
  - Route traffic for /app1/* to Target Group A.
  - Route traffic for /app2/* to Target Group B.

### 5. Query String Rules:

- **Description**: Specific to ALB, allows routing based on query string parameters.

- **Use Cases**: Route traffic based on specific query parameters or values.

- **Example**:
  - Route traffic with ?product=123 to Target Group A.
  - Route traffic with ?category=books to Target Group B.

### 6. Redirect Rules:

- **Description**: Specific to ALB, allows creating URL redirects.

- **Use Cases**: Implement URL redirections for specific paths or URLs.

- **Example**:
  - Redirect /old-path to /new-path with a 301 status code.

### 7. Fixed Response Rules:

- **Description**: Specific to ALB, allows generating fixed responses for specific requests.

- **Use Cases**: Send predefined responses for certain requests, like custom error pages.

- **Example**:
  - Return a custom "Not Found" HTML page with a 404 status code.

These are some of the key rule types and elements you can configure in an AWS Application Load Balancer (ALB) to control how traffic is routed to your target groups and instances. Rules 
play a critical role in enabling advanced routing, load balancing, and customization for your applications hosted on AWS ELB.

## Cross-Zone Load Balancing Cheat Sheet:

### 1. Cross-Zone Load Balancing:

- **Description**: Cross-Zone Load Balancing is a feature available for both Application Load Balancers (ALB) and Network Load Balancers (NLB).

- **Purpose**: Distributes traffic evenly across all instances in all availability zones rather than segregating traffic to specific zones.

- **Benefits**:
  - Improves traffic distribution and fault tolerance.
  - Helps in distributing traffic to instances across availability zones for better utilization.

### 2. Enabling Cross-Zone Load Balancing:

- **ALB**:
  - Enabled by default; no action needed.

- **NLB**:
  - Enabled when creating or modifying an NLB.

## Stickiness:

### 1. Stickiness:

- **Description**: Stickiness, also known as session affinity, ensures that a client's requests are directed to the same target instance during the session.

- **Purpose**: Useful for applications that require session persistence, such as maintaining state for shopping carts or user sessions.

- **Types**:
  - **Application Load Balancer (ALB)**: Supports sticky sessions using cookies.
  - **Classic Load Balancer (CLB)**: Supports cookie-based session stickiness.

### 2. Cookie-Based Stickiness (ALB):

- **Key Components**:
  - **Duration**: Set the time (in seconds) for the stickiness. The session remains sticky during this period.
  - **Stickiness Policy**: ALB generates a session cookie for clients and associates requests with the same cookie value to the same target instance.

- **Use Cases**:
  - E-commerce websites to maintain a consistent shopping cart experience.
  - Applications requiring user-specific sessions.

### 3. Application Load Balancer (ALB) Stickiness:

- **Enabling Stickiness**:
  - When configuring a target group, set the **stickiness policy**.

### 4. Classic Load Balancer (CLB) Stickiness:

- **Description**: Classic Load Balancers support stickiness using a custom-generated cookie.

- **Enabling Stickiness**:
  - When configuring the CLB, set the **Enable** option for session stickiness.
  - Specify the **stickiness policy name**, **cookie name**, and the **cookie expiration period**.

- **Use Cases**:
  - Applications that need custom stickiness policies and session management.

### 5. NLB Stickiness:

- **Description**: NLBs do not support built-in session stickiness. You must implement stickiness at the application level for NLBs.

- **Use Cases**:
  - Implement custom session management at the application layer.

Stickiness and Cross-Zone Load Balancing are essential features when configuring AWS Elastic Load Balancers to ensure that traffic is distributed effectively and that user sessions are maintained 
consistently. Use these features based on your application's requirements for session persistence and load balancing.

## Step-by-Step Guide for Configuring ELB:

### Application Load Balancer (ALB):

1. **Create an Application Load Balancer**:
    - Choose the ALB class.
    - Define the listener configuration (HTTP/HTTPS).
    - Specify security groups and subnets.
    - Configure routing rules.

2. **Create Target Groups**:
    - Define target group settings (e.g., protocol and port).
    - Register instances or IP addresses to the target group.

3. **Set Up Listener Rules**:
    - Define routing rules based on path, host, or query parameters.
    - Map rules to target groups.

4. **Configure Health Checks**:
    - Define health check settings for target group.

5. **Access Logs and Monitoring**:
    - Enable access logs and CloudWatch monitoring for the ALB.

### Network Load Balancer (NLB):

1. **Create a Network Load Balancer**:
    - Choose the NLB class.
    - Define listener configuration (TCP/UDP).
    - Specify security groups and subnets.

2. **Create Target Groups**:
    - Define target group settings (e.g., protocol and port).
    - Register instances or IP addresses to the target group.

3. **Configure Load Balancer Settings**:
    - Set cross-zone load balancing, idle timeout, and client IP preservation.

4. **Access Logs and Monitoring**:
    - Enable access logs and CloudWatch monitoring for the NLB.

### Classic Load Balancer (Deprecated - Consider migrating):

1. **Create a Classic Load Balancer**:
    - Choose the Classic Load Balancer class.
    - Define listener configuration (HTTP/HTTPS or TCP/UDP).
    - Specify security groups and subnets.

2. **Register Instances**:
    - Register EC2 instances with the load balancer.

3. **Configure Health Checks**:
    - Define health check settings for instances.

4. **Set Up Listener Policies**:
    - Define stickiness policies, if required.

5. **Access Logs and Monitoring**:
    - Enable access logs and CloudWatch monitoring for the Classic Load Balancer.

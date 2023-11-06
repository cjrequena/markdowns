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

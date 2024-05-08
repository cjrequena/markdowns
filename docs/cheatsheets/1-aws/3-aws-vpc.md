---
layout: default
title:  aws-vpc
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-vpc
nav_order: 3
---
# AWS VPC Reference Guideline
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Introduction to AWS VPC
AWS Virtual Private Cloud (VPC) allows you to create isolated network environments within the AWS cloud. It enables you to launch resources in a logically isolated section of the AWS Cloud.

## Components of a VPC
A VPC consists of:
- IP Address Range (CIDR block)
- Subnets
- Route Tables
- Network ACLs
- Security Groups

### IP Addressing in VPC
VPCs use CIDR notation to define IP address ranges. For example: `10.0.0.0/16`.

### Subnets
Subnets in Amazon Virtual Private Cloud (VPC) are logical segments of IP address ranges within a VPC. They play a crucial
role in segmenting and isolating resources, providing network security, and enabling communication between instances. 

### Route Tables
Route tables in AWS are used to determine where network traffic is directed within a virtual private cloud (VPC). They 
act as a set of rules, called routes, that specify how packets should be forwarded. Each VPC comes with a default route 
table, and you can create additional custom route tables to suit your networking needs.

**Components of a Route Table:**
1. **Routes**: Routes define the destination network and the target where traffic should be sent. This target can be an internet gateway, a virtual private gateway (for VPN connections), a VPC peering connection, a network interface, or a NAT gateway.

2. **Associations**: Route tables are associated with subnets within a VPC. Each subnet can only be associated with one route table at a time. If a subnet isn't explicitly associated with a custom route table, it uses the default route table of the VPC.

3. **Propagation**: Route tables can propagate routes from VPN connections and Direct Connect gateways. This allows you to route traffic from your VPC to your corporate network or other networks connected to your AWS infrastructure.

**Example Scenario:**
Let's consider a simple scenario where you have a VPC with two subnets: one public subnet and one private subnet. You want instances in the public subnet to have internet access, while instances in the private subnet should only communicate with each other and with resources within your VPC.

1. **Create a Custom Route Table**:
    - First, you create a custom route table named "PublicRouteTable" for your public subnet. This route table will have a route pointing to an internet gateway (IGW) to enable internet access.

2. **Define Routes**:
    - In the "PublicRouteTable", you add a route with a destination of `0.0.0.0/0` (which represents all internet-bound traffic) and the target as the internet gateway ID.

3. **Associate with Public Subnet**:
    - Next, you associate the "PublicRouteTable" with your public subnet.

4. **Create a Default Route Table**:
    - Your VPC already has a default route table. This default route table will be used for your private subnet unless you specify otherwise.

5. **Configure Private Subnet**:
    - Since you want your private subnet to communicate only within the VPC, you don't need an internet gateway. Instances in the private subnet can communicate with each other and resources in the VPC.

6. **Associate with Private Subnet**:
    - The default route table is automatically associated with any subnet created within the VPC that isn't explicitly associated with another route table. So, your private subnet will use the default route table by default.

### Network ACLs
Network Access Control Lists (ACLs) in Amazon Virtual Private Cloud (VPC) act as stateless firewalls for controlling 
inbound and outbound traffic at the subnet level. They provide an additional layer of security beyond security groups 
and allow you to define rules that filter traffic based on source and destination IP addresses, ports, and protocols.

### Security Groups
Security Groups in Amazon Virtual Private Cloud (VPC) act as virtual firewalls that control inbound and outbound traffic 
for instances at instance level. They are a fundamental component of network security within AWS and play a crucial role 
in enforcing access control policies.

## Subnet Configuration
### Public Subnets
Public subnets have direct route to the internet via an Internet Gateway. Typically used for resources that need public accessibility.

### Private Subnets
Private subnets have no direct route to the internet. Used for resources requiring additional security.

## Network Access Control
### Security Groups
Act as a virtual firewall for EC2 instances. Control inbound/outbound traffic at the instance level.

### Network ACLs
Control traffic entering/exiting the subnet. Stateless and rule-based. Applied at the subnet level.

## VPC Peering
Connect VPCs together, allowing resources to communicate across VPCs securely using private IP addresses.

## VPN and Direct Connect
Establish secure connections between on-premises networks and VPCs using VPN (Virtual Private Network) or Direct Connect.

## Internet Gateway and NAT Gateway
- **Internet Gateway**: Allows resources in public subnets to connect to the internet.
- **NAT Gateway**: Enables instances in private subnets to access the internet while preventing inbound traffic.

## VPC Endpoints
Enable private connectivity between VPCs and supported AWS services (e.g., S3, DynamoDB) without using public internet.

## VPC Flow Logs
Capture information about IP traffic going to and from network interfaces in your VPC. Useful for troubleshooting and security analysis.

## Best Practices
- Properly design your VPC architecture for scalability, fault tolerance, and security.
- Use security groups and network ACLs effectively to control traffic.
- Utilize public and private subnets based on resource requirements.

## Troubleshooting
- Check security group and network ACL rules.
- Review route tables and internet/NAT gateway configurations.
- Analyze VPC flow logs for traffic insights.

## Further Resources
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [VPC Cheat Sheet](https://cjrequena.com/markdowns/docs/aws/aws-cli#aws-cli-commands-for-amazon-vpc)

---


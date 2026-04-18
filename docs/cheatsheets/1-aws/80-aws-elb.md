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

**AWS Elastic Load Balancer (ELB)** is Amazon Web Services' fully managed load balancing service. It automatically distributes incoming
application traffic across multiple targets (EC2 instances, containers, IP addresses, Lambda functions) within an AWS VPC.

Key characteristics:
- Automatically routes traffic to healthy targets.
- Monitors the health of backend resources.
- Scales based on demand.
- Supports SSL/TLS termination, sticky sessions, security group integration, and access logs.
- Integrates seamlessly with other AWS services (Auto Scaling, ECS, WAF, ACM, CloudWatch, etc.).

## ELB Classes

### Application Load Balancer (ALB)

- Operates at **layer 7** (HTTP/HTTPS).
- Load balancing to multiple HTTP applications across machines (target groups).
- Load balancing to multiple applications on the same machine (e.g., containers).
- Support for HTTP/2 and WebSockets.
- Supports redirects (e.g., from HTTP to HTTPS).
- Advanced routing based on host, path, query parameters, HTTP headers, and HTTP methods.
- Has a port mapping feature to redirect to a dynamic port in ECS (useful for running multiple containers on the same instance).
- Fixed hostname (xxx.region.elb.amazonaws.com).
- The application servers do not see the IP of the client directly:
  - True client IP is inserted in the header `X-Forwarded-For`.
  - Port via `X-Forwarded-Port` and protocol via `X-Forwarded-Proto`.
- Supports AWS WAF integration for web application firewall protection.
- Supports desync mitigation mode (monitor, defensive, strictest) to protect against HTTP desync attacks.
- Best suited for web applications and microservices.

### Network Load Balancer (NLB)

- Operates at **layer 4** (TCP/UDP/TLS).
- Handles millions of requests per second with ultra-low latency (~100 ms vs ~400 ms for ALB).
- Has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IPs).
- Preserves the source IP of the client.
- Supports security groups (added in 2023).
- Supports AWS PrivateLink to expose services to other VPCs privately.
- Best for applications requiring extreme performance and low latency.

### Gateway Load Balancer (GWLB)

- Operates at **layer 3** (Network layer) — IP Packets.
- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS.
- Examples: Firewalls, intrusion detection/prevention systems, deep packet inspection, payload manipulation.
- Combines the following functions:
  - **Transparent network gateway** — single entry/exit for all traffic.
  - **Load balancer** — distributes traffic to your virtual appliances.
- Uses the GENEVE protocol on port 6081.

### Classic Load Balancer (CLB) — Deprecated

- Supports both HTTP/HTTPS and TCP/UDP traffic.
- Provides basic load balancing capabilities.
- **Deprecated** — migrate to ALB or NLB for enhanced features and performance.

## Health Checks

Health checks allow ELB to determine whether targets are healthy and available to receive traffic.

- Configured at the **target group level** (ALB, NLB, GWLB) or **instance level** (CLB).
- Key settings:
  - **Protocol**: HTTP, HTTPS, TCP, gRPC.
  - **Path**: Health check endpoint (e.g., `/health`).
  - **Interval**: Time between health checks (default: 30 seconds).
  - **Timeout**: Time to wait for a response (default: 5 seconds).
  - **Healthy threshold**: Number of consecutive successes to mark as healthy (default: 5).
  - **Unhealthy threshold**: Number of consecutive failures to mark as unhealthy (default: 2).
- ALB/NLB health checks support HTTP response code matching (e.g., `200-299`).
- If all targets are unhealthy, ELB will route traffic to all targets (fail-open behavior).

## AWS Target Groups

Target groups route requests to registered targets using the protocol and port number you specify.

- You can register a target with multiple target groups.
- Health checks are configured per target group.
- Each target group is associated with only one load balancer.

**Target types:**

- **EC2 Instances** (can be managed by an Auto Scaling Group) — HTTP.
- **ECS tasks** (managed by ECS) — HTTP.
- **Lambda functions** — HTTP request is translated to a JSON event.
- **IP addresses** — must be private IPs.
- **ALB** — if the load balancer is a Network Load Balancer (NLB → ALB chaining).
- ALB can route to multiple target groups.

## AWS ELB Rules

Rules are specific to **Application Load Balancers (ALB)** and define how incoming traffic is routed.

### Listener Rules

- **Priority**: Numeric order of evaluation (lower numbers evaluated first). Default rule is evaluated last.
- **Conditions**: Path, host header, HTTP headers, HTTP methods, query string, source IP.
- **Actions**: Forward to target group, redirect, fixed response, authenticate (Cognito/OIDC).

### Routing Types

| Routing Type | Description | Example |
|---|---|---|
| **Host-based** | Route based on the `Host` header | `api.example.com` → Target Group A |
| **Path-based** | Route based on URL path | `/api/*` → Target Group B |
| **Query string** | Route based on query parameters | `?platform=mobile` → Target Group C |
| **HTTP header** | Route based on any HTTP header | Custom header routing |
| **HTTP method** | Route based on request method | `POST` → Target Group D |
| **Source IP** | Route based on client IP CIDR | `10.0.0.0/8` → Target Group E |

### Action Types

- **Forward**: Route to one or more target groups (supports weighted routing for blue/green deployments).
- **Redirect**: Return HTTP 301/302 redirect (e.g., HTTP → HTTPS).
- **Fixed response**: Return a custom HTTP response (e.g., 404 page).
- **Authenticate**: Integrate with Amazon Cognito or any OIDC-compliant IdP.

## Cross-Zone Load Balancing

Distributes traffic evenly across all registered targets in all enabled AZs, rather than only within the same AZ.

| ELB Type | Default | Inter-AZ Data Charges |
|---|---|---|
| **ALB** | Enabled by default | No charges. Can be disabled at target group level. |
| **NLB / GWLB** | Disabled by default | You pay charges if enabled. |
| **CLB** | Disabled by default | No charges. |

## Stickiness

Stickiness (session affinity) ensures that a client's requests are directed to the same target during a session.

### ALB Stickiness

- **Application-based cookie** (custom cookie name): Generated by the application. Cookie name must not use `AWSALB`, `AWSALBAPP`, or `AWSALBTG`.
- **Duration-based cookie** (`AWSALB`): Generated by the ALB. Configurable duration (1 second to 7 days).
- Configured at the **target group level**.
- Enabling stickiness may cause imbalanced load distribution.

### NLB Stickiness

- Source IP-based stickiness at the target group level.
- Connections from the same client IP are routed to the same target.

### CLB Stickiness

- Supports duration-based and application-controlled cookie stickiness.

## Connection Draining / Deregistration Delay

Allows the load balancer to gracefully complete in-flight requests to targets being deregistered.

- Called **Connection Draining** in CLB, **Deregistration Delay** in ALB/NLB.
- **Default**: 300 seconds (configurable: 0–3600 seconds).
- **Set to 0** to disable and deregister targets immediately.
- During draining: no new connections are sent to the target, existing connections are allowed to complete.
- After draining completes (or timeout expires), the target is fully deregistered.

## SSL/TLS on AWS ELB

- An SSL/TLS certificate allows traffic between clients and the load balancer to be encrypted in transit.
- SSL = Secure Sockets Layer (older). TLS = Transport Layer Security (newer, current standard).
- Public SSL/TLS certificates are issued by Certificate Authorities (CA): Comodo, Symantec, GoDaddy, GlobalSign, DigiCert, Let's Encrypt, etc.
- Certificates have an expiration date and must be renewed.

### SSL/TLS Termination

- ELB terminates SSL/TLS connections, offloading encryption/decryption from backend instances.
- ELB uses X.509 certificates (SSL/TLS server certificate).
- Manage certificates using **AWS Certificate Manager (ACM)** or upload your own.
- HTTPS Listener:
  - You must specify a **default certificate**.
  - You can add an optional list of certificates to support multiple domains.
  - Clients can use **SNI** (Server Name Indication) to specify the hostname they reach.

### Server Name Indication (SNI)

- Solves the problem of loading multiple SSL/TLS certificates onto one server (to serve multiple websites).
- Requires the client to indicate the hostname of the target server in the initial TLS handshake.
- The server finds the correct certificate, or returns the default one.
- Supported by **ALB**, **NLB**, and **CloudFront**. Not supported by CLB.

### Security Policies

- ELB provides predefined SSL security policies with recommended protocol versions and ciphers.
- Supports TLS 1.2 and TLS 1.3.
- You can select a policy based on your security and compliance requirements.
- Custom security policies are available for CLB only.

### Optional Mutual TLS (mTLS)

- ALB supports mutual TLS authentication (client certificates) for additional security.
- Can be configured in **verify mode** (verify but don't enforce) or **passthrough mode**.
- Requires a trust store with CA certificates.

## SSL/TLS Certificate with AWS Certificate Manager (ACM)

1. Open the **ACM Console** → Click **Request a certificate**.
2. Choose **Public** or **Private** certificate type.
3. Enter domain names (e.g., `example.com`, `*.example.com`).
4. Choose validation method:
   - **DNS validation** (recommended) — create a CNAME record in your DNS.
   - **Email validation** — receive validation email at domain-associated addresses.
5. Confirm and submit the request.
6. Complete validation → certificate status changes to **Issued**.
7. Use the certificate with ELB, CloudFront, API Gateway, etc.

> **Note**: ACM certificates used with ELB are free. ACM handles automatic renewal for DNS-validated certificates.

## Configuring SSL/TLS on an ALB

1. Create or select an ALB in the **EC2 → Load Balancers** console.
2. Add an **HTTPS listener** (port 443).
3. Select or upload an **SSL/TLS certificate** (ACM recommended).
4. Choose an **SSL security policy**.
5. Configure a **target group** to forward decrypted traffic to.
6. (Optional) Add an HTTP listener (port 80) with a **redirect action** to HTTPS.
7. Test the configuration using the ALB's DNS name.

## Access Logs

- ELB can log all requests to an **Amazon S3 bucket**.
- Logs include: timestamp, client IP, latencies, request paths, server responses, target IP.
- Useful for debugging, compliance, and traffic analysis.
- **ALB/CLB**: Access logs are disabled by default. Enable in load balancer attributes.
- **NLB**: Access logs are also available but capture TCP/TLS connection-level information.
- Logs are stored in a compressed format and delivered every 5 minutes.
- The S3 bucket must have the correct **bucket policy** to allow ELB to write logs.

## AWS WAF Integration

- **ALB** integrates with **AWS WAF** (Web Application Firewall) to protect against common web exploits.
- You can attach a WAF Web ACL to an ALB to filter requests based on:
  - IP addresses, geo-location, request size, string matching, regex patterns.
  - Rate-based rules for DDoS protection.
  - AWS Managed Rules for common threats (SQL injection, XSS, etc.).
- WAF is **not supported** on NLB, GWLB, or CLB.

## AWS PrivateLink (NLB)

- **NLB** supports **AWS PrivateLink** to expose services privately to other VPCs.
- Consumers create a **VPC Endpoint** (Interface type) that connects to the NLB via PrivateLink.
- Traffic stays within the AWS network — no internet gateway, NAT, or VPN required.
- Useful for SaaS providers, shared services, and cross-account access.

## ELB Pricing

ELB pricing varies by type and is based on:

| Component | ALB | NLB | GWLB | CLB |
|---|---|---|---|---|
| **Hourly charge** | Per ALB-hour | Per NLB-hour | Per GWLB-hour | Per CLB-hour |
| **Usage metric** | LCU (Load Balancer Capacity Units) | NLCU (NLB Capacity Units) | GLCU (GWLB Capacity Units) | Data processed |

- **LCU/NLCU/GLCU** dimensions: new connections, active connections, processed bytes, rule evaluations.
- Cross-AZ data transfer charges apply for NLB and GWLB when cross-zone load balancing is enabled.
- No charge for partial LCU/NLCU/GLCU hours.

> For detailed pricing, use the [AWS Pricing Calculator](https://calculator.aws).

## Quotas and Constraints

| Quota | ALB | NLB | GWLB | CLB |
|---|---|---|---|---|
| Max Listeners | 100 | 50 | 1 | 100 |
| Max Target Groups | 100 | 50 | 1 | N/A |
| Max Rules per Listener | 100 (default) | N/A | N/A | N/A |
| Idle Timeout | 60s (1–4000s) | 350s (configurable) | N/A | 60s |

> **Note**: Quotas are subject to change. Check the [AWS ELB Quotas documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-limits.html) for latest values. Some quotas can be increased via service quota requests.

## Quick Setup Reference

### ALB Setup

1. Create ALB → define listeners (HTTP/HTTPS), security groups, subnets.
2. Create target groups → register targets (instances, IPs, Lambda).
3. Configure listener rules → path/host-based routing to target groups.
4. Configure health checks per target group.
5. (Optional) Attach WAF Web ACL, enable access logs, configure stickiness.

### NLB Setup

1. Create NLB → define listeners (TCP/UDP/TLS), subnets, Elastic IPs.
2. Create target groups → register targets.
3. Configure health checks, cross-zone load balancing, and client IP preservation.
4. (Optional) Enable access logs, configure PrivateLink.

### ALB with Auto Scaling Group

1. Create ALB with target group and health checks.
2. Create Auto Scaling Group → select AMI, instance type, scaling policies.
3. Attach the ALB target group to the Auto Scaling Group.
4. Configure scaling policies (CPU, network, custom CloudWatch metrics).
5. Monitor via CloudWatch → fine-tune scaling policies based on traffic patterns.

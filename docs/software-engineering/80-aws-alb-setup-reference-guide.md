---
layout: default
title: aws-alb-setup-reference-guide
parent: software-engineering
nav_order: 80
---
# AWS Application Load Balancer (ALB) Setup Reference Guide
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Introduction

This guide provides a detailed, step-by-step reference for setting up an AWS Application Load Balancer (ALB). It covers the full lifecycle from prerequisites through production-ready configuration, including HTTPS, health checks, routing rules, WAF integration, and monitoring.

An ALB operates at **layer 7** (HTTP/HTTPS) and is best suited for web applications, microservices, and container-based architectures. It supports advanced routing, SSL/TLS termination, WebSockets, HTTP/2, and integration with AWS WAF, Cognito, and ECS.

---

## Prerequisites

Before creating an ALB, ensure the following are in place:

| Prerequisite | Description |
|---|---|
| **VPC** | A VPC with at least **two public subnets** in different Availability Zones. |
| **Security Groups** | A security group for the ALB allowing inbound traffic on ports 80 (HTTP) and/or 443 (HTTPS). |
| **Target Instances/Containers** | EC2 instances, ECS tasks, IP addresses, or Lambda functions to receive traffic. |
| **SSL/TLS Certificate** | An ACM certificate (or uploaded certificate) if configuring HTTPS. |
| **IAM Permissions** | Sufficient IAM permissions to create and manage ELB, EC2, ACM, and S3 resources. |
| **DNS (Optional)** | A Route 53 hosted zone or external DNS for custom domain mapping. |

---

## Step 1 — Create the ALB

### AWS Console

1. Open the **EC2 Console** → **Load Balancers** → **Create Load Balancer**.
2. Select **Application Load Balancer**.
3. Configure basic settings:
   - **Name**: A descriptive name (e.g., `my-app-alb`).
   - **Scheme**: `internet-facing` (public) or `internal` (private).
   - **IP address type**: `ipv4` or `dualstack` (IPv4 + IPv6).
4. Under **Network mapping**:
   - Select the **VPC**.
   - Select at least **two subnets** in different AZs.
5. Click **Next** to proceed to security group configuration.

### AWS CLI

```bash
aws elbv2 create-load-balancer \
  --name my-app-alb \
  --subnets subnet-0abc1234 subnet-0def5678 \
  --security-groups sg-0abc1234 \
  --scheme internet-facing \
  --type application \
  --ip-address-type ipv4
```

### Terraform

```hcl
resource "aws_lb" "app" {
  name               = "my-app-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = [aws_subnet.public_a.id, aws_subnet.public_b.id]

  enable_deletion_protection = true

  tags = {
    Environment = "production"
  }
}
```

---

## Step 2 — Configure Security Groups

The ALB requires a security group that allows inbound traffic from clients and outbound traffic to targets.

### ALB Security Group

| Rule | Type | Protocol | Port | Source |
|---|---|---|---|---|
| Inbound | HTTP | TCP | 80 | `0.0.0.0/0` (or restricted CIDR) |
| Inbound | HTTPS | TCP | 443 | `0.0.0.0/0` (or restricted CIDR) |
| Outbound | All traffic | All | All | Target security group |

### Target Security Group

| Rule | Type | Protocol | Port | Source |
|---|---|---|---|---|
| Inbound | Application | TCP | Application port (e.g., 8080) | ALB security group ID |

> Targets should **only** accept traffic from the ALB security group, not directly from the internet.

### Terraform

```hcl
resource "aws_security_group" "alb" {
  name   = "alb-sg"
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_security_group" "targets" {
  name   = "targets-sg"
  vpc_id = aws_vpc.main.id

  ingress {
    from_port       = 8080
    to_port         = 8080
    protocol        = "tcp"
    security_groups = [aws_security_group.alb.id]
  }
}
```

---

## Step 3 — Create Target Groups

Target groups define where the ALB routes traffic and how it checks target health.

### AWS Console

1. **EC2 Console** → **Target Groups** → **Create target group**.
2. Choose target type: **Instances**, **IP addresses**, **Lambda function**, or **ALB**.
3. Configure:
   - **Name**: e.g., `my-app-tg`.
   - **Protocol**: HTTP or HTTPS.
   - **Port**: Application port (e.g., 8080).
   - **VPC**: Same VPC as the ALB.
   - **Protocol version**: HTTP/1.1 or HTTP/2 (or gRPC).
4. Configure health checks (see Step 4).
5. Register targets (instances, IPs, or Lambda functions).

### AWS CLI

```bash
aws elbv2 create-target-group \
  --name my-app-tg \
  --protocol HTTP \
  --port 8080 \
  --vpc-id vpc-0abc1234 \
  --target-type instance \
  --health-check-protocol HTTP \
  --health-check-path /health \
  --health-check-interval-seconds 30 \
  --healthy-threshold-count 3 \
  --unhealthy-threshold-count 2

aws elbv2 register-targets \
  --target-group-arn arn:aws:elasticloadbalancing:region:account:targetgroup/my-app-tg/1234567890 \
  --targets Id=i-0abc1234 Id=i-0def5678
```

### Terraform

```hcl
resource "aws_lb_target_group" "app" {
  name        = "my-app-tg"
  port        = 8080
  protocol    = "HTTP"
  vpc_id      = aws_vpc.main.id
  target_type = "instance"

  health_check {
    enabled             = true
    path                = "/health"
    port                = "traffic-port"
    protocol            = "HTTP"
    healthy_threshold   = 3
    unhealthy_threshold = 2
    interval            = 30
    timeout             = 5
    matcher             = "200"
  }

  deregistration_delay = 300
}
```

---

## Step 4 — Configure Health Checks

Health checks determine whether targets are healthy and eligible to receive traffic.

| Setting | Recommended Value | Description |
|---|---|---|
| **Protocol** | HTTP | Protocol used for health checks. |
| **Path** | `/health` | Endpoint that returns health status. |
| **Port** | `traffic-port` | Same port as the target receives traffic on. |
| **Healthy threshold** | 3 | Consecutive successes to mark healthy. |
| **Unhealthy threshold** | 2 | Consecutive failures to mark unhealthy. |
| **Interval** | 30 seconds | Time between health checks. |
| **Timeout** | 5 seconds | Time to wait for a response. |
| **Success codes** | `200` (or `200-299`) | HTTP status codes indicating health. |

### Health Check Best Practices

- The health check endpoint should be **lightweight** — avoid database queries or heavy computation.
- Return a **200 OK** with a simple JSON body (e.g., `{"status": "UP"}`).
- Set the **unhealthy threshold lower** than the healthy threshold for faster failure detection.
- If using a **grace period** (with ASG), ensure it exceeds the application startup time.
- If all targets are unhealthy, the ALB will route traffic to all targets (fail-open behavior).

---

## Step 5 — Create Listeners

Listeners check for connection requests from clients using the protocol and port you configure.

### HTTP Listener (Port 80) — Redirect to HTTPS

```bash
aws elbv2 create-listener \
  --load-balancer-arn arn:aws:elasticloadbalancing:region:account:loadbalancer/app/my-app-alb/1234567890 \
  --protocol HTTP \
  --port 80 \
  --default-actions Type=redirect,RedirectConfig="{Protocol=HTTPS,Port=443,StatusCode=HTTP_301}"
```

### HTTPS Listener (Port 443) — Forward to Target Group

```bash
aws elbv2 create-listener \
  --load-balancer-arn arn:aws:elasticloadbalancing:region:account:loadbalancer/app/my-app-alb/1234567890 \
  --protocol HTTPS \
  --port 443 \
  --ssl-policy ELBSecurityPolicy-TLS13-1-2-2021-06 \
  --certificates CertificateArn=arn:aws:acm:region:account:certificate/cert-id \
  --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:account:targetgroup/my-app-tg/1234567890
```

### Terraform

```hcl
resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.app.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type = "redirect"
    redirect {
      port        = "443"
      protocol    = "HTTPS"
      status_code = "HTTP_301"
    }
  }
}

resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.app.arn
  port              = 443
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-TLS13-1-2-2021-06"
  certificate_arn   = aws_acm_certificate.app.arn

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}
```

---

## Step 6 — Configure Listener Rules (Routing)

Listener rules define how the ALB routes requests beyond the default action. Rules are evaluated in **priority order** (lowest number first).

### Path-Based Routing

Route requests to different target groups based on the URL path.

```bash
aws elbv2 create-rule \
  --listener-arn arn:aws:elasticloadbalancing:region:account:listener/app/my-app-alb/1234567890/listener-id \
  --priority 10 \
  --conditions Field=path-pattern,Values='/api/*' \
  --actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:account:targetgroup/api-tg/1234567890
```

### Host-Based Routing

Route requests based on the `Host` header (useful for multi-tenant or multi-domain setups).

```bash
aws elbv2 create-rule \
  --listener-arn arn:aws:elasticloadbalancing:region:account:listener/app/my-app-alb/1234567890/listener-id \
  --priority 20 \
  --conditions Field=host-header,Values='api.example.com' \
  --actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:account:targetgroup/api-tg/1234567890
```

### Terraform — Multiple Rules

```hcl
resource "aws_lb_listener_rule" "api" {
  listener_arn = aws_lb_listener.https.arn
  priority     = 10

  condition {
    path_pattern {
      values = ["/api/*"]
    }
  }

  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.api.arn
  }
}

resource "aws_lb_listener_rule" "admin" {
  listener_arn = aws_lb_listener.https.arn
  priority     = 20

  condition {
    host_header {
      values = ["admin.example.com"]
    }
  }

  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.admin.arn
  }
}
```

### Routing Conditions Summary

| Condition | Field | Example |
|---|---|---|
| Path pattern | `path-pattern` | `/api/*`, `/images/*` |
| Host header | `host-header` | `api.example.com` |
| HTTP header | `http-header` | `X-Custom-Header: value` |
| HTTP method | `http-request-method` | `POST`, `GET` |
| Query string | `query-string` | `?platform=mobile` |
| Source IP | `source-ip` | `10.0.0.0/8` |

---

## Step 7 — SSL/TLS Certificate Setup

### Request a Certificate via ACM

1. Open **ACM Console** → **Request a certificate**.
2. Choose **Public certificate**.
3. Enter domain names (e.g., `example.com`, `*.example.com`).
4. Select **DNS validation** (recommended).
5. Create the CNAME record in Route 53 or your DNS provider.
6. Wait for status to change to **Issued**.

### AWS CLI

```bash
aws acm request-certificate \
  --domain-name example.com \
  --subject-alternative-names "*.example.com" \
  --validation-method DNS
```

### Attach Additional Certificates (SNI)

For serving multiple domains on a single ALB, add additional certificates to the HTTPS listener:

```bash
aws elbv2 add-listener-certificates \
  --listener-arn arn:aws:elasticloadbalancing:region:account:listener/app/my-app-alb/1234567890/listener-id \
  --certificates CertificateArn=arn:aws:acm:region:account:certificate/additional-cert-id
```

> ALB uses **Server Name Indication (SNI)** to select the correct certificate based on the hostname in the TLS handshake.

### SSL Security Policy Selection

| Policy | TLS Versions | Use Case |
|---|---|---|
| `ELBSecurityPolicy-TLS13-1-2-2021-06` | TLS 1.2 + 1.3 | Recommended for most workloads. |
| `ELBSecurityPolicy-TLS13-1-3-2021-06` | TLS 1.3 only | Maximum security, modern clients only. |
| `ELBSecurityPolicy-2016-08` | TLS 1.0 + 1.1 + 1.2 | Legacy compatibility (not recommended). |

---

## Step 8 — Enable Access Logs

Access logs capture detailed information about every request processed by the ALB.

### Enable via Console

1. **EC2 Console** → **Load Balancers** → Select ALB → **Attributes** → **Edit**.
2. Enable **Access logs**.
3. Specify the **S3 bucket** and optional prefix.

### AWS CLI

```bash
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn arn:aws:elasticloadbalancing:region:account:loadbalancer/app/my-app-alb/1234567890 \
  --attributes Key=access_logs.s3.enabled,Value=true \
               Key=access_logs.s3.bucket,Value=my-alb-logs-bucket \
               Key=access_logs.s3.prefix,Value=alb-logs
```

### S3 Bucket Policy

The S3 bucket must allow the ELB service to write logs. Replace `REGION` and `ELB_ACCOUNT_ID` with the [ELB account ID for your region](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/enable-access-logging.html).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ELB_ACCOUNT_ID:root"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-alb-logs-bucket/alb-logs/AWSLogs/ACCOUNT_ID/*"
    }
  ]
}
```

### Terraform

```hcl
resource "aws_lb" "app" {
  # ... other configuration ...

  access_logs {
    bucket  = aws_s3_bucket.alb_logs.id
    prefix  = "alb-logs"
    enabled = true
  }
}
```

---

## Step 9 — Enable WAF Integration

Attach an AWS WAF Web ACL to the ALB to protect against common web exploits.

### AWS CLI

```bash
aws wafv2 associate-web-acl \
  --web-acl-arn arn:aws:wafv2:region:account:regional/webacl/my-web-acl/acl-id \
  --resource-arn arn:aws:elasticloadbalancing:region:account:loadbalancer/app/my-app-alb/1234567890
```

### Terraform

```hcl
resource "aws_wafv2_web_acl_association" "alb" {
  resource_arn = aws_lb.app.arn
  web_acl_arn  = aws_wafv2_web_acl.main.arn
}
```

### Recommended WAF Managed Rules

| Rule Group | Protection |
|---|---|
| `AWSManagedRulesCommonRuleSet` | General web exploits (XSS, path traversal, etc.) |
| `AWSManagedRulesSQLiRuleSet` | SQL injection attacks. |
| `AWSManagedRulesKnownBadInputsRuleSet` | Known malicious inputs (Log4j, etc.) |
| `AWSManagedRulesAmazonIpReputationList` | Blocks IPs with poor reputation. |
| `AWSManagedRulesBotControlRuleSet` | Bot detection and mitigation. |

---

## Step 10 — Configure Stickiness (Optional)

Enable session stickiness if your application requires requests from the same client to reach the same target.

### Duration-Based (ALB-Generated Cookie)

```bash
aws elbv2 modify-target-group-attributes \
  --target-group-arn arn:aws:elasticloadbalancing:region:account:targetgroup/my-app-tg/1234567890 \
  --attributes Key=stickiness.enabled,Value=true \
               Key=stickiness.type,Value=lb_cookie \
               Key=stickiness.lb_cookie.duration_seconds,Value=86400
```

### Application-Based (Custom Cookie)

```bash
aws elbv2 modify-target-group-attributes \
  --target-group-arn arn:aws:elasticloadbalancing:region:account:targetgroup/my-app-tg/1234567890 \
  --attributes Key=stickiness.enabled,Value=true \
               Key=stickiness.type,Value=app_cookie \
               Key=stickiness.app_cookie.cookie_name,Value=MY_APP_COOKIE \
               Key=stickiness.app_cookie.duration_seconds,Value=86400
```

> Stickiness may cause uneven load distribution. Use only when required by the application (e.g., session state stored in memory).

---

## Step 11 — Configure Monitoring with CloudWatch

### Key ALB CloudWatch Metrics

| Metric | Description | Alarm Threshold (Example) |
|---|---|---|
| `RequestCount` | Total number of requests processed. | Baseline + 50% |
| `TargetResponseTime` | Average time for targets to respond. | > 2 seconds |
| `HTTPCode_ELB_5XX_Count` | 5xx errors generated by the ALB. | > 0 |
| `HTTPCode_Target_5XX_Count` | 5xx errors returned by targets. | > 10/min |
| `HealthyHostCount` | Number of healthy targets. | < desired count |
| `UnHealthyHostCount` | Number of unhealthy targets. | > 0 |
| `ActiveConnectionCount` | Total active concurrent connections. | Capacity planning |
| `RejectedConnectionCount` | Connections rejected (max connections reached). | > 0 |

### Create a CloudWatch Alarm (AWS CLI)

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name alb-5xx-errors \
  --metric-name HTTPCode_ELB_5XX_Count \
  --namespace AWS/ApplicationELB \
  --statistic Sum \
  --period 300 \
  --threshold 10 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 1 \
  --dimensions Name=LoadBalancer,Value=app/my-app-alb/1234567890 \
  --alarm-actions arn:aws:sns:region:account:my-alerts-topic
```

---

## Step 12 — DNS Configuration

Point your custom domain to the ALB using Route 53 or an external DNS provider.

### Route 53 Alias Record (Recommended)

```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id Z1234567890 \
  --change-batch '{
    "Changes": [{
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "app.example.com",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "Z35SXDOTRQ7X7K",
          "DNSName": "my-app-alb-1234567890.us-east-1.elb.amazonaws.com",
          "EvaluateTargetHealth": true
        }
      }
    }]
  }'
```

### Terraform

```hcl
resource "aws_route53_record" "app" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "app.example.com"
  type    = "A"

  alias {
    name                   = aws_lb.app.dns_name
    zone_id                = aws_lb.app.zone_id
    evaluate_target_health = true
  }
}
```

> Use **Alias records** (not CNAME) for ALBs in Route 53 — they are free and support zone apex domains.

---

## Step 13 — Integration with Auto Scaling Group

Attach the ALB target group to an ASG so new instances are automatically registered.

### AWS CLI

```bash
aws autoscaling attach-load-balancer-target-groups \
  --auto-scaling-group-name my-asg \
  --target-group-arns arn:aws:elasticloadbalancing:region:account:targetgroup/my-app-tg/1234567890
```

### Terraform

```hcl
resource "aws_autoscaling_group" "app" {
  name                = "my-asg"
  min_size            = 2
  max_size            = 10
  desired_capacity    = 2
  vpc_zone_identifier = [aws_subnet.private_a.id, aws_subnet.private_b.id]
  target_group_arns   = [aws_lb_target_group.app.arn]
  health_check_type   = "ELB"

  launch_template {
    id      = aws_launch_template.app.id
    version = "$Latest"
  }
}
```

> Set `health_check_type = "ELB"` on the ASG so that unhealthy targets (as determined by ALB health checks) are automatically replaced.

---

## Complete Terraform Example

A minimal but production-ready ALB setup:

```hcl
# ALB
resource "aws_lb" "app" {
  name               = "my-app-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = [aws_subnet.public_a.id, aws_subnet.public_b.id]

  enable_deletion_protection = true

  access_logs {
    bucket  = aws_s3_bucket.alb_logs.id
    prefix  = "alb-logs"
    enabled = true
  }
}

# Target Group
resource "aws_lb_target_group" "app" {
  name        = "my-app-tg"
  port        = 8080
  protocol    = "HTTP"
  vpc_id      = aws_vpc.main.id
  target_type = "instance"

  health_check {
    path                = "/health"
    protocol            = "HTTP"
    healthy_threshold   = 3
    unhealthy_threshold = 2
    interval            = 30
    timeout             = 5
    matcher             = "200"
  }

  deregistration_delay = 300
}

# HTTP → HTTPS Redirect
resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.app.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type = "redirect"
    redirect {
      port        = "443"
      protocol    = "HTTPS"
      status_code = "HTTP_301"
    }
  }
}

# HTTPS Listener
resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.app.arn
  port              = 443
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-TLS13-1-2-2021-06"
  certificate_arn   = aws_acm_certificate.app.arn

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}

# DNS
resource "aws_route53_record" "app" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "app.example.com"
  type    = "A"

  alias {
    name                   = aws_lb.app.dns_name
    zone_id                = aws_lb.app.zone_id
    evaluate_target_health = true
  }
}
```

---

## Troubleshooting Checklist

| Issue | Possible Cause | Resolution |
|---|---|---|
| **502 Bad Gateway** | Target is not responding or returning malformed responses. | Check target health, security groups, and application logs. |
| **503 Service Unavailable** | No healthy targets in the target group. | Verify health check configuration and target status. |
| **504 Gateway Timeout** | Target did not respond within the idle timeout. | Increase ALB idle timeout or optimize target response time. |
| **Targets stuck in `unhealthy`** | Health check path returns non-200 or times out. | Verify the health check path, port, and security group rules. |
| **Uneven traffic distribution** | Stickiness enabled or cross-zone load balancing disabled. | Review stickiness settings; ensure cross-zone is enabled. |
| **SSL handshake errors** | Certificate mismatch or expired certificate. | Verify ACM certificate covers the requested domain. |
| **Connection refused** | Security group does not allow traffic on the listener port. | Update ALB security group inbound rules. |

---

## References

- [AWS ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
- [AWS ELB Quotas](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-limits.html)
- [AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/)
- [AWS WAF Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/)
- [Terraform AWS LB Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lb)
- [AWS Pricing Calculator](https://calculator.aws)

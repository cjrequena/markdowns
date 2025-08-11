---
layout: default
title: aws-tagging-standard
parent: aws
grand_parent: cheatsheets
permalink: /docs/aws/aws-tagging-standard
nav_order: 140
---
# AWS Tagging Standard
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Tags by Category

### Technical Tags

| Tag Key              | Description                      | Example                   |
| -------------------- | -------------------------------- | ------------------------- |
| `name`               | Descriptive name of the resource | `web-server-prod`         |
| `owner`              | Person/team responsible          | `jane.doe@company.com`    |
| `availability-zone`  | AWS AZ or region                 | `us-west-2a`              |
| `service`            | AWS service type                 | `ec2`, `s3`, `rds`        |
| `lifecycle`          | Lifecycle stage                  | `active`, `archived`      |
| `version`            | Version label                    | `v1.3.2`                  |
| `created-date`       | Date created                     | `2024-11-02`              |
| `last-modified-date` | Date last modified               | `2025-04-18`              |
| `resource-id`        | Unique resource identifier       | `i-04a8b2f3ab1c12345`     |
| `environment-type`   | Environment classification       | `production`, `qa`, `dev` |

---

### Tags for Automation

| Tag Key                  | Description                       | Example                     |
| ------------------------ | --------------------------------- | --------------------------- |
| `automated`              | Managed by automation             | `yes`                       |
| `auto-scaling-group`     | Associated scaling group          | `asg-web-frontend`          |
| `automation-tool`        | Tool managing resource            | `terraform`                 |
| `scheduled-automation`   | Automation schedule participation | `yes`                       |
| `continuous-integration` | Integrated with CI/CD system      | `jenkins`, `github-actions` |
| `automated-backup`       | Backup enabled                    | `true`                      |
| `auto-shutdown`          | Scheduled shutdown enabled        | `enabled`                   |
| `auto-recovery`          | Auto-recovery setting             | `enabled`                   |
| `auto-scaling-policy`    | Scaling policy used               | `cpu-usage-threshold`       |
| `automated-monitoring`   | Monitoring tool enabled           | `cloudwatch`                |

---

### Business Tags

| Tag Key                   | Description                  | Example              |
| ------------------------- | ---------------------------- | -------------------- |
| `cost-center`             | Financial cost center        | `CC-0453-FINANCE`    |
| `department`              | Department responsible       | `engineering`        |
| `project`                 | Project or initiative        | `ecommerce-platform` |
| `business-unit`           | Business unit                | `retail-services`    |
| `customer-id`             | Associated customer          | `customer-00192`     |
| `revenue-center`          | Revenue grouping             | `rev-west-division`  |
| `business-criticality`    | Criticality level            | `high`               |
| `contract-id`             | Related contract             | `contract-2024-xyz`  |
| `service-level-agreement` | SLA attached                 | `99.99-uptime`       |
| `business-impact`         | Business consequence if down | `critical`           |

---

### Security Tags

| Tag Key                   | Description                | Example                   |
| ------------------------- | -------------------------- | ------------------------- |
| `security-classification` | Security level             | `confidential`            |
| `compliance`              | Compliance standard        | `GDPR`, `HIPAA`           |
| `backup`                  | Included in backups        | `yes`                     |
| `encryption`              | Encryption enabled         | `AES-256`                 |
| `access-control-list`     | ACL identifier             | `acl-admin-only`          |
| `security-group`          | Associated security group  | `sg-0a1234cdefb56789a`    |
| `firewall-rule`           | Related firewall rule      | `rule-web-allow-443`      |
| `vulnerability`           | Patch/vulnerability status | `patched`                 |
| `data-classification`     | Sensitivity level          | `sensitive`               |
| `security-policy`         | Applied policy             | `pci-encryption-required` |

---

### Tags for Classification

| Tag Key            | Description                          | Example                     |
| ------------------ | ------------------------------------ | --------------------------- |
| `environment`      | Resource environment                 | `production`                |
| `application`      | Application associated               | `inventory-service`         |
| `region`           | AWS region                           | `us-east-1`                 |
| `zone`             | Availability zone                    | `us-east-1b`                |
| `role`             | Component's function in architecture | `frontend`, `backend`       |
| `service-type`     | Nature of service                    | `database`, `web`, `cache`  |
| `deployment-stage` | Release maturity                     | `beta`, `release-candidate` |
| `cost-allocation`  | Cost tag group                       | `app-cost-tracking`         |
| `cost-savings`     | Included in optimization             | `yes`                       |
| `expiration-date`  | Retirement/review date               | `2025-12-31`                |

---

## Tags by Use Case

### AWS Console Organization & Resource Groups

| Use Case Key                    | Description               | Example Tag             |
| ------------------------------- | ------------------------- | ----------------------- |
| `organize-resources`            | Group by project/env/etc. | `project = crm-system`  |
| `resource-group-classification` | Resource Group setup      | `environment = staging` |

---

### Cost Allocation

| Use Case Key              | Description                     | Example Tag                      |
| ------------------------- | ------------------------------- | -------------------------------- |
| `cost-tracking`           | Track by department/project     | `cost-center = CC-104-marketing` |
| `cost-allocation-reports` | Generate Cost Explorer insights | `project = web-migration`        |

---

### Automation

| Use Case Key                    | Description                | Example Tag               |
| ------------------------------- | -------------------------- | ------------------------- |
| `automated-resource-management` | Use for Lambda/EC2 control | `auto-shutdown = enabled` |
| `automated-backup-and-recovery` | Targeted backup plans      | `automated-backup = true` |

---

### Operations Support

| Use Case Key                            | Description          | Example Tag                         |
| --------------------------------------- | -------------------- | ----------------------------------- |
| `resource-monitoring-and-alerts`        | Alerts, dashboards   | `automated-monitoring = cloudwatch` |
| `incident-response-and-troubleshooting` | Assist investigation | `owner = ops@company.com`           |

---

### Access Control

| Use Case Key                                | Description               | Example Tag                   |
| ------------------------------------------- | ------------------------- | ----------------------------- |
| `access-control-policies`                   | IAM policy filtering      | `department = legal`          |
| `resource-sharing-and-cross-account-access` | Cross-account role access | `access-level = partner-view` |

---

### Security Risk Management

| Use Case Key                                   | Description               | Example Tag                      |
| ---------------------------------------------- | ------------------------- | -------------------------------- |
| `security-group-and-nacl-rules`                | Track network policies    | `security-policy = dmz-restrict` |
| `asset-inventory-and-vulnerability-management` | Audit/patching management | `vulnerability = unpatched`      |

---

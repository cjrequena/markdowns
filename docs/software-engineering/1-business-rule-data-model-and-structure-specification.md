---
layout: default
title: template
parent: software-engineering
nav_order: 999
---
# Business Rule Data Model and Structure Specification
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Abstract

This document defines the data model and structural specifications for representing business rules in a JSON-based format. It establishes a standardized schema to describe rules, their metadata, evaluation conditions, actions, and logic. This enables consistent storage, retrieval, evaluation, and enforcement of rules across systems.

## Overview

Business rules govern the behavior of applications by defining conditional logic that triggers specific actions. These rules are modular, versioned, and managed with clear ownership and metadata. This model supports both basic and complex logic structures using standard conditions and optional [JSONLogic](https://jsonlogic.com/) for advanced scenarios.

The schema ensures:

* **Standardization** of rule structure and data
* **Flexibility** through nested conditions and logic types
* **Traceability** via metadata like creator, timestamps, and versioning
* **Auditability** through status tracking, dependencies, and tagging

---

## JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Rule",
  "type": "object",
  "properties": {
    "id": { "type": "string" },
    "title": { "type": "string" },
    "description": { "type": "string" },
    "owner": { "type": "string" },
    "created_by": { "type": "string" },
    "created_at": { "type": "string", "format": "date-time" },
    "updated_at": { "type": "string", "format": "date-time" },
    "expires_at": { "type": "string", "format": "date-time" },
    "version": { "type": "string" },
    "status": {
      "type": "string",
      "enum": ["active", "inactive", "pending", "deprecated"]
    },
    "priority": { "type": "integer" },
    "audience": {
      "type": "string",
      "enum": ["internal", "external", "beta_users", "general"]
    },
    "conditions_logic": {
      "type": "string",
      "enum": ["AND", "OR"]
    },
    "tags": {
      "type": "array",
      "items": { "type": "string" }
    },
    "depends_on": {
      "type": "array",
      "items": { "type": "string" }
    },
    "conditions": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "field": { "type": "string" },
          "operator": {
            "type": "string",
            "enum": ["IN", "NOT_IN", "EQUALS", "NOT_EQUALS", "GREATER_THAN", "LESS_THAN"]
          },
          "values": {
            "type": "array",
            "items": { "type": "string" }
          }
        },
        "required": ["field", "operator", "values"]
      }
    },
    "actions": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "type": { "type": "string" },
          "reason": { "type": "string" }
        },
        "required": ["type", "reason"]
      }
    },
    "jsonlogic": {
      "type": "object",
      "description": "JsonLogic-compatible rule object. See https://jsonlogic.com/ for syntax.",
      "additionalProperties": true
    }
  },
  "required": [
    "id",
    "title",
    "description",
    "owner",
    "created_by",
    "created_at",
    "updated_at",
    "expires_at",
    "version",
    "status",
    "priority",
    "audience",
    "conditions_logic",
    "tags",
    "depends_on",
    "conditions",
    "actions",
    "jsonlogic"
  ]
}
```

---

## JSON Generalized Example

```json
{
  "id": "<unique_rule_id>",
  "title": "<rule_title>",
  "description": "<rule_description>",
  "owner": "<owner_identifier>",
  "created_by": "<creator_identifier>",
  "created_at": "<ISO_8601_timestamp>",
  "updated_at": "<ISO_8601_timestamp>",
  "expires_at": "<ISO_8601_timestamp>",
  "version": "<version_string>",
  "status": "<<active | inactive | pending | deprecated>>",
  "priority": <integer_priority>,
  "audience": "<<internal | external | beta_users | general>>",
  "conditions_logic": "<<AND | OR>>",
  "tags": [
    "<tag1>",
    "<tag2>"
  ],
  "depends_on": [
    "<rule_id_1>",
    "<rule_id_2>"
  ],
  "conditions": [
    {
      "field": "<field_name>",
      "operator": "<<IN | NOT_IN | EQUALS | NOT_EQUALS | GREATER_THAN | LESS_THAN>>",
      "values": [
        "<value1>",
        "<value2>"
      ]
    }
  ],
  "actions": [
    {
      "type": "<action_type>",
      "reason": "<action_reason>"
    }
  ],
  "jsonlogic": {
    "in": [
      {
        "var": "<field_name>"
      },
      [
        "<value1>",
        "<value2>"
      ]
    ]
  }
}
```

---

## Field-by-Field Explanation

### 1. `id` (string)

* **Description**: A unique identifier for the rule.
* **Format**: `"R-<number>"`
* **Example**: `"R-101"`

### 2. `title` (string)

* **Description**: A concise title or name for the business rule.
* **Format**: `"string"`
* **Example**: `"Region Restriction Rule"`

### 3. `description` (string)

* **Description**: A detailed description of what the rule is designed to do and its purpose.
* **Format**: `"string"`
* **Example**: `"Deny access to experiences for users from specific regions."`

### 4. `owner` (string)

* **Description**: The team or department responsible for managing this rule.
* **Format**: `"string"`
* **Example**: `"Compliance Team"`

### 5. `created_by` (string)

* **Description**: The ID of the user who created the rule.
* **Format**: `"string"`
* **Example**: `"user-123"`

### 6. `created_at` (string)

* **Description**: The timestamp of when the rule was created.
* **Format**: `"YYYY-MM-DDTHH:MM:SSZ"`
* **Example**: `"2025-06-01T10:00:00Z"`

### 7. `updated_at` (string)

* **Description**: The timestamp of the last update to the rule.
* **Format**: `"YYYY-MM-DDTHH:MM:SSZ"`
* **Example**: `"2025-06-01T10:00:00Z"`

### 8. `expires_at` (string)

* **Description**: The timestamp of when the rule will expire or be deactivated.
* **Format**: `"YYYY-MM-DDTHH:MM:SSZ"`
* **Example**: `"2025-12-31T23:59:59Z"`

### 9. `version` (string)

* **Description**: The version of the rule.
* **Format**: `"X.X.X"`
* **Example**: `"1.0.0"`

### 10. `audience` (string)

* **Description**: The target audience for the rule (e.g., all users, specific groups, etc.).
* **Format**: `"string"`
* **Example**: `"external"`

### 11. `depends_on` (array)

* **Description**: A list of other rule IDs that this rule depends on.
* **Format**: `["string"]`
* **Example**: `["R-100"]`

### 12. `status` (string)

* **Description**: The current status of the rule. Possible values: `active`, `inactive`, `pending`, `deprecated`.
* **Format**: `"string"`
* **Example**: `"active"`

### 13. `priority` (integer)

* **Description**: The priority of the rule. Lower numbers signify higher priority.
* **Format**: `integer (1-100)`
* **Example**: `50`

### 14. `tags` (array)

* **Description**: Tags or keywords associated with the rule for easier categorization.
* **Format**: `["string"]`
* **Example**: `["compliance", "region", "access control"]`

### 15. `conditions_logic` (string)

* **Description**: The logical operator that connects the conditions. Possible values: `"AND"`, `"OR"`.
* **Format**: `"string"`
* **Example**: `"AND"`

### 16. `conditions` (array)

* **Description**: A list of conditions that determine when the rule is applied.
* **Format**: An array of objects containing `field`, `operator`, and `values`.
* **Example**:

  ```json
  [
    {
      "field": "region",
      "operator": "IN",
      "values": ["US", "UK"]
    }
  ]
  ```

### 17. `actions` (array)

* **Description**: A list of actions to be taken when the rule conditions are met.
* **Format**: An array of objects containing `type` and `reason`.
* **Example**:

  ```json
  [
    {
      "type": "DENY",
      "reason": "Region not allowed"
    }
  ]
  ```

### 18. `jsonlogic` (object)

* **Description**: A JSON object used for advanced rule logic, which enables complex condition handling.
* **Format**: A nested object with logical operations.
* **Example**:

```json=
"jsonlogic": {
  "in": [
    {
      "var": "country"
    },
    [
      "US",
      "CA"
    ]
  ]
}
```

---

## Use Cases

### Content Restriction

```json
If experience is in ["exp1", "exp2"] AND subchannel is in ["es-es"], then DENY access.
```

### Regional Compliance

```json
If user's region NOT IN ["US", "UK"], DENY experience.
```

### Feature Gating

```json
If user’s subscription tier != "premium", mask advanced features.
```

### Access Control

```json
If user is NOT in allowed roles, log event and deny action.
```

---

## Security Considerations

* **Sensitive Fields**: Be cautious when defining conditions that involve sensitive data, such as personal identifiers, financial data, or passwords. Ensure compliance with data protection regulations such as GDPR or CCPA.
* **Rule Conflicts**: Rules should be designed to avoid conflicts between them, especially when multiple rules apply to the same entity or action. Clear prioritization and dependency management are necessary to handle such conflicts.

---

## Validation Rules (Optional)

* **Expiry Checks**: Automatically deactivate expired rules.
* **Conflict Resolution**: Ensure no two active rules contradict each other on the same resource and priority level.
* **Schema Enforcement**: Validate all rule entries against the JSON Schema before deployment.
* **Version Enforcement**: Always increase the `version` field when a rule is edited.
* **Test Coverage**: Validate rule logic with test scenarios to ensure correctness before enabling.

---

## Examples

### **Example 1: Content Access Restriction Based on Region**

If a user is in a restricted region (e.g., "CN" for China), deny access to a particular content.

```json=
{
  "id": "R-101",
  "title": "Region-based Content Restriction",
  "description": "Deny access to content for users in restricted regions (e.g., China).",
  "owner": "Compliance Team",
  "created_by": "user-123",
  "created_at": "2025-06-01T10:00:00Z",
  "updated_at": "2025-06-01T10:00:00Z",
  "expires_at": "2025-12-31T23:59:59Z",
  "version": "1.0.0",
  "status": "active",
  "priority": 10,
  "audience": "external",
  "conditions_logic": "AND",
  "tags": ["region", "access", "restriction"],
  "depends_on": [],
  "conditions": [
    {
      "field": "region",
      "operator": "IN",
      "values": ["CN"]
    }
  ],
  "actions": [
    {
      "type": "DENY",
      "reason": "Region is restricted"
    }
  ],
  "jsonlogic": {
    "in": [
      { "var": "region" },
      ["CN"]
    ]
  }
}

```

---

### **Example 2: Premium Subscription Feature Access**

If a user does not have a "premium" subscription, restrict access to premium features.

```json=
{
  "id": "R-102",
  "title": "Premium Feature Gating",
  "description": "Deny access to premium features for non-premium users.",
  "owner": "Product Team",
  "created_by": "user-124",
  "created_at": "2025-06-02T11:00:00Z",
  "updated_at": "2025-06-02T11:00:00Z",
  "expires_at": "2026-06-02T23:59:59Z",
  "version": "1.1.0",
  "status": "active",
  "priority": 5,
  "audience": "external",
  "conditions_logic": "AND",
  "tags": ["subscription", "feature gating"],
  "depends_on": [],
  "conditions": [
    {
      "field": "subscription_tier",
      "operator": "NOT_IN",
      "values": ["premium"]
    }
  ],
  "actions": [
    {
      "type": "MASK",
      "reason": "Non-premium user"
    }
  ],
  "jsonlogic": {
    "!": {
      "in": [
        { "var": "subscription_tier" },
        ["premium"]
      ]
    }
  }
}

```

---

### **Example 3: User Access Based on Role**

Only allow admin users to access the admin dashboard.

```json=
{
  "id": "R-103",
  "title": "Admin Dashboard Access",
  "description": "Only allow users with 'admin' role to access the admin dashboard.",
  "owner": "Security Team",
  "created_by": "user-125",
  "created_at": "2025-06-03T12:00:00Z",
  "updated_at": "2025-06-03T12:00:00Z",
  "expires_at": "2025-12-31T23:59:59Z",
  "version": "1.0.0",
  "status": "active",
  "priority": 1,
  "audience": "internal",
  "conditions_logic": "AND",
  "tags": ["role", "access control"],
  "depends_on": [],
  "conditions": [
    {
      "field": "role",
      "operator": "IN",
      "values": ["admin"]
    }
  ],
  "actions": [
    {
      "type": "ALLOW",
      "reason": "Admin access"
    }
  ],
  "jsonlogic": {
    "in": [
      { "var": "role" },
      ["admin"]
    ]
  }
}

```

---

### **Example 4: Deny User Access Based on Age**

If the user's age is less than 18, deny access to a restricted content page.

```json=
{
  "id": "R-104",
  "title": "Age-based Content Restriction",
  "description": "Deny access to content if user is under 18 years old.",
  "owner": "Compliance Team",
  "created_by": "user-126",
  "created_at": "2025-06-04T13:00:00Z",
  "updated_at": "2025-06-04T13:00:00Z",
  "expires_at": "2025-12-31T23:59:59Z",
  "version": "1.0.0",
  "status": "active",
  "priority": 15,
  "audience": "general",
  "conditions_logic": "AND",
  "tags": ["age", "restriction", "content"],
  "depends_on": [],
  "conditions": [
    {
      "field": "age",
      "operator": "LESS_THAN",
      "values": ["18"]
    }
  ],
  "actions": [
    {
      "type": "DENY",
      "reason": "User under age restriction"
    }
  ],
  "jsonlogic": {
    "<": [
      { "var": "age" },
      18
    ]
  }
}

```

---

### **Example 5: Offer Discount for Specific Countries**

Provide a discount offer for users from the United States or Canada.

```json=
{
  "id": "R-105",
  "title": "Country-based Discount Offer",
  "description": "Offer a 10% discount to users from the United States and Canada.",
  "owner": "Marketing Team",
  "created_by": "user-127",
  "created_at": "2025-06-05T14:00:00Z",
  "updated_at": "2025-06-05T14:00:00Z",
  "expires_at": "2025-12-31T23:59:59Z",
  "version": "1.0.0",
  "status": "active",
  "priority": 20,
  "audience": "external",
  "conditions_logic": "OR",
  "tags": ["offer", "discount", "country"],
  "depends_on": [],
  "conditions": [
    {
      "field": "country",
      "operator": "IN",
      "values": ["US", "CA"]
    }
  ],
  "actions": [
    {
      "type": "DISCOUNT",
      "reason": "Special offer for US and Canada"
    }
  ],
  "jsonlogic": {
    "in": [
      { "var": "country" },
      ["US", "CA"]
    ]
  }
}

```

---

### **Example 6: Deny Access if User is Suspended**

Deny access to the platform if the user’s account status is "suspended."

```json=
{
  "id": "R-106",
  "title": "Suspended Account Access Denial",
  "description": "Deny platform access if the user's account status is 'suspended'.",
  "owner": "Security Team",
  "created_by": "user-128",
  "created_at": "2025-06-06T15:00:00Z",
  "updated_at": "2025-06-06T15:00:00Z",
  "expires_at": "2025-12-31T23:59:59Z",
  "version": "1.0.0",
  "status": "active",
  "priority": 30,
  "audience": "internal",
  "conditions_logic": "AND",
  "tags": ["account", "suspended", "access control"],
  "depends_on": [],
  "conditions": [
    {
      "field": "account_status",
      "operator": "EQUALS",
      "values": ["suspended"]
    }
  ],
  "actions": [
    {
      "type": "DENY",
      "reason": "Account is suspended"
    }
  ],
  "jsonlogic": {
    "==": [
      { "var": "account_status" },
      "suspended"
    ]
  }
}

```

### **Example 7: Deny Access if User is Suspended**

If experience is in ["exp1", "exp2"] AND subchannel is in ["es-es"], then DENY access.

```json=
{
  "id": "rule_001",
  "title": "Deny Access for Specific Experience and Subchannel",
  "description": "This rule denies access when experience is in ['exp1', 'exp2'] AND subchannel is in ['es-es'].",
  "owner": "owner_123",
  "created_by": "creator_456",
  "created_at": "2025-06-01T00:00:00Z",
  "updated_at": "2025-06-01T00:00:00Z",
  "expires_at": "2025-12-31T23:59:59Z",
  "version": "1.0",
  "status": "active",
  "priority": 1,
  "audience": "general",
  "conditions_logic": "AND",
  "tags": [
    "access_control",
    "experience_rules"
  ],
  "depends_on": [],
  "conditions": [
    {
      "field": "experience",
      "operator": "IN",
      "values": [
        "exp1",
        "exp2"
      ]
    },
    {
      "field": "subchannel",
      "operator": "IN",
      "values": [
        "es-es"
      ]
    }
  ],
  "actions": [
    {
      "type": "DENY",
      "reason": "Access denied due to restricted experience and subchannel."
    }
  ],
  "jsonlogic": {
    "and": [
      {
        "in": [
          { "var": "experience" },
          ["exp1", "exp2"]
        ]
      },
      {
        "in": [
          { "var": "subchannel" },
          ["es-es"]
        ]
      }
    ]
  }
}

```


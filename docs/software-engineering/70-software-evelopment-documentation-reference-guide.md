---
layout: default
title: cloudevents-across-distributed-system-reference-architecture
parent: software-engineering
nav_order: 60
---

# CloudEvents Across Distributed System - Reference Architecture
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Abstract

This document presents a reference architecture and best practices for using CloudEvents as a standardized event format across diverse messaging systems including Kafka, AWS SNS, Kinesis, and SQS. 

It outlines how to structure events, handle metadata and extensions, and ensure schema validation consistency in heterogeneous environments.

This document defines a dual-mode CloudEvents reference architecture for integrating Apache Kafka with AWS messaging systems (SNS, SQS, Kinesis).

The recommended approach is to adopt:

1. **CloudEvents in binary mode** When Event-Router / Broker does have native support for message headers. **In binary mode**, event metadata **(CloudEvents attributes)** are sent as message headers, and event data is sent as the message body. Optimizing for performance and metadata flexibility.

2. **CloudEvents in structured mode**, When Event-Router / Broker does not have native support for message headers. Encapsulating all event metadata and the payload in a single JSON object. This ensures maximum portability across transports..

A conversion pipeline is introduced to map binary CloudEvents from Kafka to structured CloudEvents for downstream AWS systems, and vice versa. This enables seamless event replication, transformation, and routing across environments while preserving schema validation, event traceability, and event semantics.

To address schema validation needs, it is advised to validate only the `data` portion of the CloudEvent against a registered schema in a schema registry (e.g., Confluent, AWS Glue), using the `type`, `dataschema` to identify the appropriate schema.

---

## CloudEvents

**[CloudEvents](https://github.com/cloudevents/spec)** is a CNCF specification for describing event data in a consistent, structured way across services and platforms. 

It’s transport-neutral and supports formats like JSON, Avro, and Protobuf. 

### Core Components

- **Producer**: The service that generates the event.
- **Event Router / Broker**: Middleware like native Eventing, Kafka, etc.
- **Consumer**: Services that subscribe to and handle events.
- **Event Format**: JSON/Avro with CloudEvents envelope
- **Transport Binding**: HTTP, Kafka, MQTT, NATS, SNS, Kinesis, SQS

### JSON Example 1

```json
{
  "specversion": "1.0",
  "type": "com.example.object.created",
  "source": "/mycontext",
  "id": "A234-1234-1234",
  "time": "2023-01-02T12:34:56Z",
  "datacontenttype": "application/json",
  "data": {
    "object_id": "abc123",
    "status": "created"
  }
}
```

### JSON Example 2

```json
{
  "specversion": "1.0",
  "id": "a1b2c3d4-e5f6-7890-abcd-1234567890ef",
  "source": "/hotel-booking-service",
  "type": "com.example.hotel.booking.created",
  "subject": "booking/67890",
  "time": "2025-05-17T14:25:35Z",
  "datacontenttype": "application/json",
  "dataschema": "https://schemas.example.com/hotel-booking-created-v1.json",
  "data": {
    "booking_id": "67890",
    "hotel_id": "hotel-123",
    "guest": {
      "first_name": "Alice",
      "last_name": "Doe",
      "email": "alice@example.com"
    },
    "check_in_date": "2025-06-01",
    "check_out_date": "2025-06-05",
    "room_type": "Deluxe",
    "total_amount": 850.00,
    "currency": "USD"
  }
}
```

### Explanation of Fields

| **Attribute**   | **Required** | **Description**                                                           |
| --------------- | ------------ | ------------------------------------------------------------------------- |
| specversion     | ✅ Yes        | Version of the CloudEvents spec used                                      |
| id              | ✅ Yes        | Unique identifier for the event instance                                  |
| source          | ✅ Yes        | URI-like identifier of the event source (e.g., microservice)              |
| type            | ✅ Yes        | The type of event — use reverse-DNS naming (com.example.\*)               |
| subject         | ❗ Optional   | Describes the subject or resource within the source (e.g., booking/67890) |
| time            | ❗ Optional   | Timestamp of when the event occurred (RFC 3339)                           |
| datacontenttype | ❗ Optional   | Media type of data payload (usually application/json)                     |
| dataschema      | ❗ Optional   | URI to the schema that describes the data payload                         |
| data            | ❗ Optional   | The actual event payload (domain-specific)                                |

---

## Setting Extensions

**Extensions** are custom attributes added to a CloudEvent outside the standard ones (like `id`, `type`, etc.).

### How to Set

Just add key-value pairs at the top level for **structure-mode** or at header level for **binary-mode**

```json
{
  "specversion": "1.0",
  "type": "com.example.event",
  "source": "/source",
  "id": "1234",
  "myextension": "custom-value",
  "data": { ... }
}
```

### Best Practices:**

- Use lower-case keys with no special characters
- Use namespaced keys to avoid collisions, e.g., `com.company.traceid`
- Only use for routing, filtering, tracing—**not** for business data

### Custom Extensions

| Extension Name  | Type      | Description                                                               |
| --------------- | --------- | ------------------------------------------------------------------------- |
| `ext_environment` | `string`  | Deployment environment (e.g., `"prod"`, `"staging"`, `"dev"`)             |
| `ext_region`    | `string`  | Cloud region where the event originated (e.g., `"us-west-2"`)             |
| `ext_traceid`   | `string`  | Trace ID for distributed tracing (OpenTelemetry or Zipkin format)         |
| `ext_spanid`    | `string`  | Span ID for tracing within a transaction                                  |
| `ext_bookingchannel` | `string`  | Channel used to create the booking (e.g., `"mobile"`, `"web"`, `"agent"`) |
| `ext_userid`    | `string`  | ID of the authenticated user who made the booking                         |
| `ext_correlationid` | `string`  | Correlation ID for linking events across services                         |
| `ext_retrycount` | `integer` | Number of retries attempted for delivering this event                     |
| `ext_tenantid`  | `string`  | ID of the customer or tenant (for multi-tenant SaaS platforms)            |
| `ext_operationtype` | `string`  | Operation being performed (`"create"`, `"update"`, `"cancel"`)            |
| `ext_locale`    | `string`  | Locale or language setting of the client (e.g., `"en-US"`)                |
| `ext_currencycode` | `string`  | Currency used in the transaction (e.g., `"USD"`, `"EUR"`)                 |
| `ext_appversion` | `string`  | Version of the client app or API                                          |
| `ext_eventpriority` | `string`  | Priority of the event (`"low"`, `"normal"`, `"high"`)                     |
| `ext_processingnode` | `string`  | Logical or physical name of the node that processed the event             |
| `ext_schemaid`  | `string`  | Schema registry ID for data validation                                    |

---

## Binary Mode - Kafka

### Key Features

- Format: **Binary mode CloudEvents**
- Metadata and extensions → **Kafka headers**
- Payload → Raw `data` as message body
- High-throughput, minimal duplication

### Example Kafka Message

- **Headers:**

```
ce_specversion: 1.0
ce_id: a1b2c3d4-e5f6-7890-abcd-1234567890ef
ce_source: /hotel-booking-service
ce_type: com.example.hotel.booking.created
ce_subject: booking/67890
ce_time: 2025-05-17T14:25:35Z
ce_datacontenttype: application/json
ce_dataschema: https://schemas.example.com/hotel-booking-created-v1.json
```

- **Payload:**

```json
{
  "booking_id": "67890",
  "hotel_id": "hotel-123",
  "guest": {
    "first_name": "Alice",
    "last_name": "Doe",
    "email": "alice@example.com"
  },
  "check_in_date": "2025-06-01",
  "check_out_date": "2025-06-05",
  "room_type": "Deluxe",
  "total_amount": 850.00,
  "currency": "USD"
}
```

### Schema Validation Strategy

- Use `ce_dataschema` to fetch schema.
- Validate payload (`data`) only.

---

## Structured Mode - SNS, SQS, Kinesis

### Key Features

- Format: **Structured mode CloudEvents (JSON)**
- Entire event in the **message body**
- Optional: Use **MessageAttributes** for routing in SNS/SQS

### Example SNS/SQS/Kinesis Message

```json
{
  "specversion": "1.0",
  "id": "a1b2c3d4-e5f6-7890-abcd-1234567890ef",
  "source": "/hotel-booking-service",
  "type": "com.example.hotel.booking.created",
  "subject": "booking/67890",
  "time": "2025-05-17T14:25:35Z",
  "datacontenttype": "application/json",
  "dataschema": "https://schemas.example.com/hotel-booking-created-v1.json",
  "data": {
    "bookingId": "67890",
    "hotelId": "hotel-123",
    "guest": {
      "firstName": "Alice",
      "lastName": "Doe",
      "email": "alice@example.com"
    },
    "checkInDate": "2025-06-01",
    "checkOutDate": "2025-06-05",
    "roomType": "Deluxe",
    "totalAmount": 850.00,
    "currency": "USD"
  }
}
```

**Optional MessageAttributes (SNS/SQS):**

```json
{
  "ce_type": "com.example.user.created",
  "ce_source": "/user-service"
}
```

### Schema Validation Strategy

- Use `dataschema` to fetch schema.
- Validate payload (`data`) only.

---

## Mapping & Pipeline: (Binary → Structured)

### Kafka → SNS/SQS/Kinesis (Binary → Structured)

**Transformation Steps:**

1. Consume Kafka message
2. Extract headers → Map to CloudEvents fields
3. Extract payload → Inject into `data` field
4. Assemble structured CloudEvent JSON
5. Publish to SNS/SQS/Kinesis

**Tools/Stack Options:**

- Use **Apache Camel**, **Debezium**, **Kafka Connect**, **Lambda**, or custom processor.
- Implement a transformation layer:
  - KafkaConsumer → Transformer → AWS SDK Publisher.

**Example Tools:**

- Camel Kafka Connector with CloudEvents support
- Kafka Streams processor + AWS SDK

---

### SNS/SQS/Kinesis → Kafka (Structured → Binary)

**Transformation Steps:**

1. Receive structured CloudEvent as JSON
2. Parse payload
3. Extract CloudEvent fields → Map to Kafka headers
4. Insert `data` → Kafka message value
5. Produce to Kafka using CloudEvents binary mode

**Tools/Stack Options:**

- AWS Lambda or Kinesis Data Firehose
- API Gateway + transformer microservice
- Custom consumer → KafkaProducer with header mapping

---

### Mapping Logic Reference

| CloudEvent Field | Kafka Header (Binary)      | Structured JSON Field      |
| ---------------- | -------------------------- | -------------------------- |
| `id`             | `ce_id`                    | `id`                       |
| `type`           | `ce_type`                  | `type`                     |
| `source`         | `ce_source`                | `source`                   |
| `time`           | `ce_time`                  | `time`                     |
| `specversion`    | `ce_specversion`           | `specversion`              |
| `data`           | **Message body**           | `data`                     |
| Extensions       | `ce_extension_<name>`      | top-level extension fields |
| Schema reference | `ce_dataschema` (optional) | `dataschema`               |

---

### Strategic Benefits

| Benefit                  | Kafka Binary Mode       | Structured Mode (SNS/SQS)      |
| ------------------------ | ----------------------- | ------------------------------ |
| Performance              | ✅ High (no duplication) | ⚠️ Slightly larger payloads    |
| Portability              | ⚠️ Kafka-specific       | ✅ Compatible everywhere        |
| Metadata support         | ✅ Full via headers      | ⚠️ Limited (MessageAttributes) |
| Schema validation ready  | ✅ Per message type      | ✅ Per message type             |
| Event mesh compatibility | ✅ With mapping layer    | ✅ Native                       |

---

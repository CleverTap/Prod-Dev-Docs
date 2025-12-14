---
title: Ingesting Nested Objects via CleverTap APIs
excerpt: >-
  Learn how to configure schemas and send structured JSON data through the
  CleverTap REST API using Nested Objects.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Nested Objects allow developers to represent structured user data directly within user profile payloads. This feature supports up to three levels of nesting and is available through the REST API. 

For more information, refer to [Nested Objects](https://docs.clevertap.com/docs/nested-objects).

> 📘 Public Beta
> 
> This feature is released in Public Beta. For more information about this feature, contact your Customer Success Manager or CleverTap Support.

## Prerequisites

Before using Nested Objects, check the following:

- The feature is enabled for your CleverTap account.
- Your account schema supports object and array data types.

## Sending Structured Data

You can send nested fields in JSON payloads through our REST API.

The following example shows how to represent a user with multiple insurance policies and beneficiaries using nested objects:

**Example: User Profile**

```json
{
  "Name": "Jane Doe",
  "Policies": [
    {
      "PolicyID": "POL123",
      "Type": "Health",
      "PremiumAmount": 1200,
      "Beneficiaries": [
        {
          "Name": "John Doe",
          "Email": "john@example.com"
        },
        {
          "Name": "Emily Doe",
          "Email": "emily@example.com"
        }
      ]
    },
    {
      "PolicyID": "POL456",
      "Type": "Life",
      "PremiumAmount": 800,
      "Beneficiaries": []
    }
  ]
}
```

## Ingestion and Validation

Follow these guidelines to maintain consistent and valid ingestion:

**Ingestion**

- Keep schema definitions identical across all data sources.
- Avoid sending empty or null objects. CleverTap automatically excludes them during ingestion.

**Validation**

- Validate all JSON payloads before sending them to the API.
- Monitor your account’s ingestion logs for errors or dropped fields.

## System Considerations

CleverTap enforces limits to ensure consistent performance and reliable ingestion of nested data. The following table lists the limits applied during nested object ingestion to ensure system stability and consistent data handling.

| Limit Type                   | Maximum Value                             |
| ---------------------------- | ----------------------------------------- |
| Nesting depth                | 3 levels                                  |
| Payload size                 | 32 KB per profile or event                |
| Object or array keys at root | 5                                         |
| Reserved fields              | `identity`, `email`, and `ts` remain flat |

If a limit is exceeded, CleverTap drops extra elements or raises an error in the response.

## Error Handling

CleverTap logs ingestion and validation errors in the API responses.

The following table lists common ingestion and validation errors with the resolutions:

| Error             | Description                                         | Resolution                                                 |
| ----------------- | --------------------------------------------------- | ---------------------------------------------------------- |
| Oversized payload | Exceeds the allowed payload size or nesting depth.  | Reduce nesting levels or split data into smaller payloads. |
| Dropped fields    | Some nested keys were ignored due to system limits. | Verify nesting depth, key count, and object size.          |
| Slow queries      | Too many nested field conditions in segmentation.   | Simplify queries and limit nested filters.                 |

When errors occur, review the API response message and check the CleverTap dashboard for ingestion logs.

## Troubleshooting

Use the following steps to diagnose issues when sending or querying nested data:

- **Schema mismatch:** Check that your schema and payload structure match exactly.
- **Oversized payload:** Reduce nested array lengths or split data into multiple events.
- **Dropped fields:** Verify the number of keys and nesting depth are within limits.
- **Slow queries:** Limit nested filters and use simpler segmentation rules.

# FAQs

### Can we ingest Nested Properties in Custom Events?

No. Nested Properties are currently supported only for user properties via API ingestion. Custom event properties do not support nested structures at this time. 

### Can I use Nested Objects in SDK, CSV, or SFTP uploads?

No. Nested Objects are supported only through the REST API. 

### How does CleverTap handle invalid JSON?

Invalid JSON objects are rejected. The corresponding event appears in the error stream.

### What happens to arrays exceeding system limits?

CleverTap automatically drops excess array elements.

### Are reserved fields supported in nested structures?

No. Fields such as `identity`, `email`, and `ts` always remain flat for consistency.

### Can nested data be exported through APIs?

No. Nested objects are not currently included in API-based data exports.

### Does this feature impact API performance?

When used within supported limits, Nested Objects do not affect overall API performance. However, deeply nested or complex queries may take slightly longer to process.
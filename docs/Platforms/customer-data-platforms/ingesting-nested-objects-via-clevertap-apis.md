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

[Nested Objects](https://docs.clevertap.com/docs/nested-objects-for-user-properties-and-custom-event-properties) allow developers to represent structured data directly within user profiles and custom event payloads. This feature supports up to three levels of nesting and is available through CleverTap's REST API. You can use Nested Objects to model complex user attributes, such as policies or subscriptions, and structured event-level data, such as cart details, product line items, or transaction metadata. This enables scalable data modeling without increasing the number of flat properties in your account.

For example payloads, refer to the following:

* [Example: Upload User Profile with Nested Objects](doc:ingesting-nested-objects-via-clevertap-apis#example-upload-user-profile-with-nested-objects)
* [Example: Upload Event with Nested Objects](doc:ingesting-nested-objects-via-clevertap-apis#example-upload-event-with-nested-objects)

> 📘 Public Beta
>
> This feature is released in Public Beta. For more information about this feature, contact your Customer Success Manager or CleverTap Support.

## Prerequisites

Before using Nested Objects, check the following:

* The feature is enabled for your CleverTap account.
* Your account schema supports object and array data types.

## Send Nested Objects in the Upload User Profile API

To send Nested Objects as user properties, include the nested JSON directly inside the `profileData` object in the [Upload User Profile API](doc:upload-user-profiles-api) payload. You do not need to flatten nested fields.

The Upload User Profile API payload accepts Nested Objects and arrays within `profileData`, provided they conform to the supported structure rules and validation limits.

### Example: Upload User Profile with Nested Objects

The following example shows how to send a user profile with multiple insurance policies and nested beneficiaries. The nested object is added inside the `profileData` field of the [Upload User Profile API ](doc:upload-user-profiles-api) payload.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        
        "Name": "Jane Doe", // level 0
        "Customer Type": "Platinum",
        "InsuranceDetails": {    
          "Policy": [ // level 1
            { 
              "PolicyID": "POL123", // level 2
              "Type": "Health",
              "PremiumAmount": 1200,
              "Beneficiary": [
                {
                  "Name": "John Doe", // level 3
                  "Email": "john@example.com",
                  "DOB": "2020-01-20"
                },
                {
                  "Name": "Emily Doe",
                  "Email": "emily@example.com",
                  "DOB": "2018-05-12"
                }
              ]
            }
          ]
        }
      }
    }
  ]
}
```

In this payload, you can find the following:

* `profileData` contains both standard profile properties and Nested Objects.
* The `InsuranceDetails.Policy` array is a Nested Object stored inside `profileData`.

## Send Nested Objects in the Upload Events API

To send Nested Objects as custom event properties, include the nested JSON directly inside the `evtData` object in the [Upload Events API ](doc:upload-events-api) payload.

The Upload Events API  payload accepts Nested Objects and arrays within `evtData`, provided they conform to the supported structure rules and validation limits.

### Example: Upload Event with Nested Objects

The following example shows how to send a `Proceed To Pay` event with nested event properties. The nested object is added inside the `evtData` field of the[ Upload Events API ](doc:upload-events-api)payload.

```json
{
  "d": [
    {
      "type": "event",
      "identity": "1001",
      "evtName": "Proceed To Pay",
      "evtData": {
        "courses_in_cart": [
          {
            "id": 10,
            "name": "Greenfield Institute of Technology",
            "courses": [
              { "id": 101, "name": "B.Tech in Computer Science and Engineering (Slot-A)" }
            ]
          },
          {
            "id": 12,
            "name": "Sunrise University, Jaipur",
            "courses": [
              { "id": 102, "name": "B.Tech in Computer Science and Engineering" },
              { "id": 103, "name": "B.E. in Computer Science and Engineering" }
            ]
          }
        ],
        "amount": 3.0,
        "page_url": "https://cd-clevertap-hz.testing.com/caf/checkout?we_checkout=true&utm_source=&utm_keywords=&utm_content=",
        "ip": "14.194.172.90",
        "payment_from": "CAF Version2"
      }
    }
  ]
}
```

Nested event data follows the same structure and validation rules as nested user properties.

> 📘 `Charged.Items` is Reserved
>
> Nested Objects do not apply to the Charged event’s Items key. The `Charged.Items` field is a reserved implementation and supports an array of objects by design.
>
> For more information, refer to [Charged as a Special Event](https://developer.clevertap.com/docs/events#what-makes-charged-a-special-event).

## Ingestion and Validation

Follow these guidelines to maintain consistent and valid ingestion:

**Ingestion**

* Keep schema definitions identical across all data sources.
* Avoid sending empty or null objects. CleverTap automatically excludes them during ingestion.

**Validation**

* Validate all JSON payloads before sending them to the API.
* Monitor your account’s ingestion logs for errors or dropped fields.

## System Considerations

CleverTap enforces limits to ensure consistent performance and reliable ingestion of nested data. The following table lists the limits applied during nested object ingestion to ensure system stability and consistent data handling.

| Limit Type                                  | Maximum Value                                                                                                               |
| :------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------- |
| Nesting depth                               | 3 levels                                                                                                                    |
| Payload size                                | 32 KB; Applies to the total size of profileData (profile uploads) or evtData (event uploads), including all nested content. |
| Root-level keys (objects or arrays)         | Maximum 5                                                                                                                   |
| Nested elements inside an array             | Maximum 100                                                                                                                 |
| Nested keys inside an object                | Maximum 100                                                                                                                 |
| Reserved fields for user properties         | `identity`, `email`, and `timestamp` remain flat                                                                            |
| Reserved fields for custom event properties | `Charged.Items`                                                                                                             |

For custom events, CleverTap rejects the request if nested event properties are empty (null or empty objects).

## Error Handling

CleverTap logs ingestion and validation errors in the API responses.

The following table lists common ingestion and validation errors with the resolutions:

| Error             | Description                                               | Resolution                                                 |
| ----------------- | --------------------------------------------------------- | ---------------------------------------------------------- |
| Oversized payload | Exceeds the allowed payload size or maximum nesting depth | Reduce nesting levels or split data into smaller payloads. |
| Dropped fields    | Some nested keys were ignored due to system limits.       | Verify nesting depth, key count, and object size.          |
| Slow queries      | Too many nested field conditions in segmentation.         | Simplify queries and limit nested filters.                 |

When errors occur, review the API response message and check the CleverTap dashboard for ingestion logs.

## Troubleshooting

Use the following steps to diagnose issues when sending or querying nested data:

* _Schema Mismatch_: Check that your schema and payload structure match exactly.
* _Oversized Payload_: Reduce nested array lengths or split data into multiple events.
* _Dropped Fields_: Verify the number of keys and nesting depth are within limits.
* _Slow Queries_: Limit nested filters and use simpler segmentation rules.

# FAQs

### Can I use Nested Objects in the SDK?

Yes. Nested Objects are supported in the CleverTap SDK for both iOS and Android.

### How does CleverTap handle invalid JSON?

Invalid JSON objects are rejected. The corresponding event appears in the error stream.

### What happens to arrays exceeding system limits?

CleverTap automatically drops excess array elements.

### Are reserved fields supported in nested structures?

No. Fields such as `identity`, `email`, and `timestamp` always remain flat.

### How does `Charged.Items` support Nested Objects?

`Charged.Items` is a reserved field. It already supports an array of objects as part of CleverTap’s built-in Charged event structure.

You cannot modify its structure or apply nested rules to it. If you need additional structured data, send it as separate event properties outside `Charged.Items`.

### Can nested data be exported through APIs?

No. Nested Objects are not currently included in API-based data exports.

### Does this feature impact API performance?

Nested Objects do not affect ingestion performance when used within supported limits. However, segmentation and query performance can degrade if multiple filters are applied to deeply nested fields or large nested arrays. To maintain performance, limit nested conditions and avoid large nested arrays in fields that are frequently queried.

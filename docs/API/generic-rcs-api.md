---
title: Generic RCS API
excerpt: Learn how to configure RCS API
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

This document describes how messaging providers can integrate with CleverTap to send Rich Communication Services (RCS) messages. The API supports the following message types:

* [Text messages](https://docs.clevertap.com/docs/rcs-message-templates#text)
* [Template Messages](https://docs.clevertap.com/docs/rcs-message-templates)
* [Rich media cards](https://docs.clevertap.com/docs/rcs-message-templates#rich-card-template)
* [Carousels](https://docs.clevertap.com/docs/rcs-message-templates#carousel)

SMS fallback is supported for all message types if the recipient is unreachable via RCS.

## Base Configuration

This section outlines the core API details required to integrate with CleverTap's RCS platform.

### Messaging API Endpoint

BSPs are requested to create an endpoint where CleverTap can send the notification payload for campaigns. Customers must ask BSPs to share the API endpoint that can accept the CleverTap Payload to save the provider in the CleverTap Dashboard.

### Authentication

Authentication details will be provided during provider setup. The process is similar to generic SMS integration.

### Headers

`Content-Type: application/json`

## Message Types

CleverTap supports four RCS message formats, each with optional SMS fallback.

### Text Message

Used for sending simple messages with optional interactive buttons such as quick replies, links, or dialers.  
**Supports:**

* Suggestions: `REPLY`, `DIAL_PHONE`, `OPEN_URL`
* SMS fallback

**Example Payload:**

```json
{
  "payloadVersion": 0.1,
  "msgId": "CT_TXT_20240315_001_ABC123",
  "to": "+911234567890",
  "callbackURL": "https://your-callback-endpoint.com/rcs/callback",
  "rcsContent": {
    "senderId": "BRAND_RCS_001",
    "customParam": [
      {
        "name": "campaign_id", 
        "value": "welcome_flow_2024"
      },
      {
        "name": "user_segment",
        "value": "premium_users"
      }
    ],
    "content": {
      "type": "TEXT",
      "text": "Welcome to our service! Would you like to explore our premium features?",
      "suggestions": [
        {
          "type": "REPLY",
          "text": "Yes, show me",
          "postbackData": "explore_premium"
        },
        {
          "type": "REPLY", 
          "text": "Maybe later",
          "postbackData": "remind_later"
        },
        {
          "type": "OPEN_URL",
          "text": "Learn More",
          "postbackData": "visit_website",
          "url": "https://example.com/features"
        }
      ]
    }
  },
  "smsContent": {
    "senderId": "BRAND001",
    "body": "Welcome to our service! Learn more at example.com/features",
    "customParam": [
      {
        "name": "campaign_id",
        "value": "welcome_flow_2024"
      }
    ]
  }
}
```

### TEMPLATE Message

Used for sending structured, pre-approved messages with dynamic fields populated using parameters.  
**Supports:**

* Template IDs and personalization parameters
* SMS fallback

**Example Payload:**

```json
{
  "payloadVersion": 0.1,
  "msgId": "CT_TMPL_20240315_002_DEF456",
  "to": "+911234567890", 
  "callbackURL": "https://your-callback-endpoint.com/rcs/callback",
  "rcsContent": {
    "senderId": "BRAND_RCS_001",
    "customParam": [
      {
        "name": "template_category",
        "value": "transactional"
      },
      {
        "name": "business_unit",
        "value": "ecommerce"
      }
    ],
    "content": {
      "type": "TEMPLATE",
      "templateId": "ORDER_CONFIRMATION_V3",
      "parameters": [
        {
          "name": "1",
          "value": "John Smith"
        },
        {
          "name": "2", 
          "value": "ORD789012"
        },
        {
          "name": "order_amount",
          "value": "₹2,499"
        },
        {
          "name": "delivery_date",
          "value": "March 20, 2024"
        }
      ]
    }
  },
  "smsContent": {
    "senderId": "BRAND001",
    "body": "Hi John Smith, your order #ORD789012 for ₹2,499 is confirmed. Expected delivery: March 20, 2024",
    "customParam": [
      {
        "name": "template_category", 
        "value": "transactional"
      }
    ]
  }
}
```

### CARD Message

Used for sending rich media with one card, including:

* Title and description
* Media Types
  * `IMAGE`: Static image content (JPG, PNG, GIF)
  * `VIDEO`: Video content (MP4, MOV, AVI)
* Up to 10 suggestions
  * Supports: `REPLY`, `DIAL_PHONE`, `OPEN_URL`
* Orientation: `HORIZONTAL`, `VERTICAL`
* Alignment: `LEFT`, `RIGHT`
* Media height settings: `SHORT`, `MEDIUM`, `TALL`
* SMS fallback

**Example Payload:**

```json
{
  "payloadVersion": 0.1,
  "msgId": "CT_CARD_20240315_003_GHI789",
  "to": "+911234567890",
  "callbackURL": "https://your-callback-endpoint.com/rcs/callback", 
  "rcsContent": {
    "senderId": "BRAND_RCS_001",
    "customParam": [
      {
        "name": "product_category",
        "value": "electronics"
      },
      {
        "name": "campaign_type",
        "value": "product_launch"
      }
    ],
    "content": {
      "type": "CARD",
      "orientation": "HORIZONTAL",
      "alignment": "LEFT",
      "cardContent": {
        "title": "Latest Smartphone Model XYZ", //Max 200 characters
        "description": "Experience cutting-edge technology with an advanced camera system, long-lasting battery, and premium design. Available in multiple colors.", //Max 2000 characters
        "media": {     
          "mediaUrl": "https://cdn.example.com/products/smartphone-xyz-hero.jpg",
          "thumbnailUrl": "https://cdn.example.com/products/smartphone-xyz-thumb.jpg", 
          "height": "MEDIUM"
        },
        "suggestions": [  //Max 10 suggestions
          {
            "type": "OPEN_URL",
            "text": "Buy Now - ₹49,999",
            "postbackData": "purchase_smartphone_xyz",
            "url": "https://store.example.com/smartphone-xyz"
          },
          {
            "type": "DIAL_PHONE",
            "text": "Call Expert",
            "postbackData": "expert_consultation",
            "phoneNumber": "+918001234567"
          },
          {
            "type": "REPLY",
            "text": "More Details",
            "postbackData": "request_details_smartphone"
          }
        ]
      }
    }
  },
  "smsContent": {
    "senderId": "BRAND001",
    "body": "Latest Smartphone Model XYZ - ₹49,999. Buy: store.example.com/smartphone-xyz Call: +918001234567",
    "customParam": [
      {
        "name": "product_category",
        "value": "electronics"  
      }
    ]
  }
}
```

### CAROUSEL Message

Used for showcasing multiple cards in a horizontally scrollable format.  
**Each card can include:**

* Title and description
* Media Types
  * `IMAGE`: Static image content (JPG, PNG, GIF)
  * `VIDEO`: Video content (MP4, MOV, AVI)
* Up to 10 suggestions
  * Supports: `REPLY`, `DIAL_PHONE`, `OPEN_URL`
* Card width: `SMALL`, `MEDIUM`
* Media height settings: `SHORT`, `MEDIUM`, `TALL`
* SMS fallback

**Example Payload:**

```json
{
  "payloadVersion": 0.1,
  "msgId": "CT_CAROUSEL_20240315_004_JKL012",
  "to": "+911234567890",
  "callbackURL": "https://your-callback-endpoint.com/rcs/callback",
  "rcsContent": {
    "senderId": "BRAND_RCS_001", 
    "customParam": [
      {
        "name": "catalog_type",
        "value": "featured_products"
      },
      {
        "name": "season",
        "value": "spring_collection_2024"
      }
    ],
    "content": {
      "type": "CAROUSEL",
      "cardWidth": "MEDIUM",
      "cardContents": [
        {
          "title": "Laptop Pro Series",
          "description": "High-performance laptop for professionals with advanced graphics and all-day battery life.",
          "media": {
            "mediaUrl": "https://cdn.example.com/products/laptop-pro-main.jpg",
            "thumbnailUrl": "https://cdn.example.com/products/laptop-pro-thumb.jpg",
            "height": "MEDIUM"
          },
          "suggestions": [
            {
              "type": "OPEN_URL",
              "text": "From ₹89,999",
              "postbackData": "laptop_pro_configure",
              "url": "https://store.example.com/laptop-pro"
            }
          ]
        },
        {
          "title": "Tablet Ultra",
          "description": "Versatile tablet with premium display, perfect for creativity and productivity tasks.",
          "media": {
            "mediaUrl": "https://cdn.example.com/products/tablet-ultra-demo.mp4", 
            "thumbnailUrl": "https://cdn.example.com/products/tablet-ultra-thumb.jpg",
            "height": "MEDIUM"
          },
          "suggestions": [
            {
              "type": "OPEN_URL",
              "text": "From ₹54,999",
              "postbackData": "tablet_ultra_purchase",
              "url": "https://store.example.com/tablet-ultra"
            }
          ]
        },
        {
          "title": "Smartwatch Elite",
          "description": "Advanced health monitoring, GPS, and smart features in a premium design.",
          "media": {
            "mediaUrl": "https://cdn.example.com/products/smartwatch-elite.jpg",
            "thumbnailUrl": "https://cdn.example.com/products/smartwatch-thumb.jpg", 
            "height": "MEDIUM"
          },
          "suggestions": [
            {
              "type": "OPEN_URL",
              "text": "₹24,999",
              "postbackData": "smartwatch_elite_shop",
              "url": "https://store.example.com/smartwatch-elite"
            }
          ]
        }
      ]
    }
  },
  "smsContent": {
    "senderId": "BRAND001",
    "body": "Featured products: Laptop Pro (₹89,999), Tablet Ultra (₹54,999), Smartwatch Elite (₹24,999). Shop at store.example.com",
    "customParam": [
      {
        "name": "catalog_type",
        "value": "featured_products"
      }
    ]
  }
}
```

## Field Specifications

The following fields are required or optional when constructing a valid RCS payload.

### Required Fields

These fields must be included in every request:

| Field                     | Type   | Description                                                                              |
| ------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| `payloadVersion`          | Number | API version (currently `0.1`)                                                            |
| `msgId`                   | String | Message identifier provided by CleverTap (must be returned in callbacks as a meta field) |
| `to`                      | String | Recipient phone number with country code (format: `[number]`)                            |
| `callbackURL`             | String | Endpoint where CleverTap expects delivery status callbacks                               |
| `rcsContent.senderId`     | String | RCS Business Messaging sender ID provided by the messaging provider                      |
| `rcsContent.content.type` | String | Message type: `TEXT`, `TEMPLATE`, `CARD`, `CAROUSEL`                                     |

### Optional Fields

Include these fields to enhance message targeting or provide fallback support:

| Field                            | Type   | Description                                                                  |
| -------------------------------- | ------ | ---------------------------------------------------------------------------- |
| `rcsContent.customParam`         | Array  | Custom key-value pairs configured during provider setup or campaign creation |
| `smsContent`                     | Object | Fallback SMS content when RCS is unavailable                                 |
| `rcsContent.content.suggestions` | Array  | Interactive buttons/actions (for `TEXT` `CARD`, and `CAROUSEL` messages)     |

## Supported Values

The following values are supported for specific fields in the payload.

### Orientation (CARD only)

| Value        | Description                                                         |
| ------------ | ------------------------------------------------------------------- |
| `HORIZONTAL` | Card content arranged horizontally with image and text side-by-side |
| `VERTICAL`   | Card content arranged vertically with image above text              |

### Alignment (CARD with HORIZONTAL orientation only)

| Value   | Description                             |
| ------- | --------------------------------------- |
| `LEFT`  | Media content aligned to the left side  |
| `RIGHT` | Media content aligned to the right side |

### Card Width (CAROUSEL only)

| Value    | Description                                 |
| -------- | ------------------------------------------- |
| `SMALL`  | Narrow card width for compact display       |
| `MEDIUM` | Standard card width for optimal readability |

### Media Height (all media types)

| Value    | Description                                        |
| -------- | -------------------------------------------------- |
| `SHORT`  | Compact media height for minimal visual impact     |
| `MEDIUM` | Standard media height for balanced presentation    |
| `TALL`   | Extended media height for prominent visual display |

### Media Types

| Value   | Description                          |
| ------- | ------------------------------------ |
| `IMAGE` | Static image content (JPG, PNG, GIF) |
| `VIDEO` | Video content (MP4, MOV, AVI)        |

### Suggestion Types

| Value        | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| `REPLY`      | Quick reply button that sends predefined text back to the sender       |
| `DIAL_PHONE` | Call action button that initiates a phone call to the specified number |
| `OPEN_URL`   | Website/app link button that opens URL in browser or app               |

## Custom Parameters

CleverTap supports metadata insertion using custom key-value pairs at two levels.

### Provider Setup (Global Parameters)

Applied to all messages sent through the provider:

* Tracking identifiers
* Provider-specific metadata
* Default campaign attributes

### Campaign Creation (Campaign-Specific Parameters)

Set in the CleverTap dashboard for specific campaigns:

* Campaign tracking codes
* Customer segments
* UTM parameters
* A/B test identifiers
* Business-specific metadata

### Common Use Cases

| Parameter                                  | Description                      |
| ------------------------------------------ | -------------------------------- |
| `campaign_id`                              | Campaign tracking identifier     |
| `user_segment`                             | Customer segment classification  |
| `utm_source`, `utm_medium`, `utm_campaign` | Marketing attribution            |
| `a_b_test_variant`                         | A/B testing group identification |
| `priority_level`                           | Message priority classification  |

## Callback Specifications

Delivery status and user interaction callbacks must be sent to the `callbackURL` specified in the original message payload. The `meta` field in callbacks must contain the original `msgId` from CleverTap's request.

The following table summarizes all supported callback event types, their trigger conditions, and additional notes to help providers correctly interpret CleverTap’s callback payloads. Each event corresponds to a specific stage in the RCS message lifecycle, from delivery and user engagement to failure reporting.

| Event Type  | Trigger Condition                                                                                                                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `delivered` | When the message is successfully delivered to the user's device.                                                                                                                                           |
| `failed`    | When message delivery fails at the provider level. Includes error `code` and `timestamp`.                                                                                                                  |
| `viewed`    | When the user views the message.                                                                                                                                                                           |
| `clicked`   | When the user clicks a URL suggestion button. Includes `url` and `userAgent`.                                                                                                                              |
| `replied`   | When the user sends the replies, such as simple text, postback, media, video, PDF, and location, for more information, refer to the [Callback Event Payload](doc:generic-rcs-api#callback-event-payloads). |

### Callback Event Payloads

```json Notification Delivered
[
  {
    "event": "delivered", 
    "data": [
      {
        "channel": "RCS",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Failed
[
  {
    "event": "failed",
    "data": [
      {
        "ts": 1710504000,
				"channel": "RCS"
        "code": "2002",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Viewed
[
  {
    "event": "viewed",
    "data": [
      {
        "channel": "RCS",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Clicked
[
  {
    "event": "clicked",
    "data": [
      {
        "ts": 1710504000,
				"channel": "RCS"
        "meta": "CT_TXT_20240315_001_ABC123",
        "userAgent": "Mozilla/5.0 (Android 12; Mobile; rv:91.0)",
        "url": "https://example.com/features",
        "shortUrl": "https://bit.ly/example123"
      }
    ]
  }
]
```
```json Notification Replied — Text Reply (Simple)
[
  {
    "event": "replied",
    "data": [
      {
        "channel": "RCS",
        "type": "text", 
        "fromPhone": "+91XXXXXXXXXX",
        "toPhone": "+91XXXXXXXXXX",
        "incomingText": "Thank you for the information",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Replied — Text Reply (with Postback)
[
  {
    "event": "replied",
    "data": [
      {
        "channel": "RCS",
        "type": "text",
        "fromPhone": "+91XXXXXXXXXX",
        "toPhone": "+91XXXXXXXXXXX",
        "incomingText": "Yes, show me",
        "postback": "explore_premium",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Replied — Media Replies (Image)
[
  {
    "event": "replied",
    "data": [
      {
        "channel": "RCS",
        "type": "image/png",
        "mediaUrl": "https://rcs-provider.com/media/user-image-abc123",
        "fromPhone": "+91XXXXXXXXXX",
        "toPhone": "+91XXXXXXXXXXX",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Replied — Media Replies (Video)
[
  {
    "event": "replied", 
    "data": [
      {
        "channel": "RCS",
        "type": "video/mp4",
        "mediaUrl": "https://rcs-provider.com/media/user-video-def456",
        "fromPhone": "+91XXXXXXXXXXX",
        "toPhone": "+91XXXXXXXXXXX",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Replied — Media Replies (PDF)
[
  {
    "event": "replied",
    "data": [
      {
        "channel": "RCS", 
        "type": "application/pdf",
        "mediaUrl": "https://rcs-provider.com/media/user-document-ghi789",
        "fromPhone": "+91XXXXXXXXXX",
        "toPhone": "+91XXXXXXXXXX", 
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```
```json Notification Replied — Media Replies (Location)
[
  {
    "event": "replied",
    "data": [
      {
        "channel": "RCS",
        "type": "location",
        "fromPhone": "+91XXXXXXXXXX",
        "toPhone": "+91XXXXXXXXXX",
        "incomingText": "My current location",
        "latitude": "19.0760",
        "longitude": "72.8777", 
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```

## Error Codes and Status Responses

This section lists all possible errors returned either synchronously (API) or asynchronously (callback).

### API-Level Errors (Provider Response)

These errors are returned immediately when the provider identifies issues during message processing.

| HTTP Status | API Error Code | Error Title                        | Description                                                          | Usage                                                        |
| :---------- | :------------- | :--------------------------------- | :------------------------------------------------------------------- | :----------------------------------------------------------- |
| 200         | 2000           | Invalid credentials                | Authentication failure, API key issues                               | Return when the auth credentials are invalid/expired         |
| 200         | 2001           | Invalid template parameters        | Missing parameters, template mismatch, incorrect template name       | Return when template validation fails                        |
| 200         | 2002           | Invalid phone number               | Phone number format errors, invalid country codes, malformed numbers | Return when phone number validation fails                    |
| 200         | 2003           | Phone number not subscribed        | User consent-related errors, opt-out status                          | Return when the user has not consented to RCS messages       |
| 200         | 2004           | RCS Disabled for the number        | Target number doesn't support RCS                                    | Return when RCS is not available for the recipient           |
| 200         | 2005           | Messaging Account Disabled         | Provider account issues, billing problems, service suspension        | Return when the provider account has issues                  |
| 200         | 2006           | Invalid Media                      | Media file issues: invalid format, size limits, broken URLs          | Return when media validation fails                           |
| 200         | 2007           | Source IP Restricted               | IP whitelisting violations                                           | Return when the request originates from a non-whitelisted IP |
| 200         | 2008           | Invalid RCS sender ID              | RCS Business Messaging sender ID errors                              | Return when the sender ID is invalid/unregistered            |
| 200         | 2009           | No Active Conversation             | Lack of an active RCS conversation window                            | Return when the RCS conversation has expired                 |
| 200         | 2011           | Outside Permitted Time Window      | Message sent outside allowed time constraints                        | Return when message timing violates regulations              |
| 200         | 2012           | Content Policy Violation           | Message content blocked due to policy violations                     | Return when content filtering detects violations             |
| 200         | 2013           | Promotional Message Limit Exceeded | Daily/hourly promotional message limits reached                      | Return when rate limits are exceeded                         |
| 200         | 2014           | Others                             | Any other provider-specific errors                                   | Return for unclassified errors                               |

### Callback-Level Error Codes (Delivery Failures)

These errors are sent via callback when delivery fails after the initial API acceptance.

| Error Title            | Description                                               | Callback Code |
| ---------------------- | --------------------------------------------------------- | ------------: |
| DND Failure            | Number registered in the Do Not Disturb registry          |           901 |
| Spam Detection         | Message blocked by carrier spam filters                   |           902 |
| Blacklist Rejection    | Number blacklisted due to previous opt-outs or complaints |           903 |
| System Error           | Internal provider system failure during processing        |           904 |
| Subscriber Error       | Recipient issues: insufficient balance, network problems  |           905 |
| DLT Header Blocked     | Sender ID blocked at DLT platform (India-specific)        |           906 |
| DLT Entity Blocked     | Entity ID issues at DLT platform (India-specific)         |           907 |
| DLT Template Blocked   | Template issues at DLT platform (India-specific)          |           908 |
| DLT Consent Error      | Missing DLT consent registration (India-specific)         |           909 |
| Invalid Subscriber     | Inactive or invalid phone number                          |           910 |
| Message Inbox Full     | Recipient's inbox storage is full                         |           912 |
| NDNC Rejected          | Number in the National Do Not Call registry               |           913 |
| Undelivered            | Delivery failed after multiple retry attempts             |           914 |
| Dropped                | Number banned or inactive (no usage for 60+ days)         |           915 |
| Expired                | Message expired after multiple delivery attempts          |           916 |
| Force Expired          | Message dropped by provider rules or client request       |           917 |
| Duplicate Message Drop | Same content sent to the same number within 30 minutes    |           918 |
| DLT Platform Failure   | DLT platform-level issues (India-specific)                |           919 |

## Error Response Format

**API Response (Immediate):**

```json API Response
{
  "success": true
}
```

```json API Response
[
  {
    "event": "failed",
    "data": [
      {
        "ts": 1710504000,
        "code": "2000",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```

**Callback Response (Delivery Failure):**

```json Callback Response
[
  {
    "event": "failed",
    "data": [
      {
        "ts": 1710504000,
        "code": "903",
        "meta": "CT_TXT_20240315_001_ABC123"
      }
    ]
  }
]
```

---
title: WhatsApp Chatbot Integration
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

CleverTap's WhatsApp Chatbot Integration allows chatbot providers to connect with WhatsApp Business Accounts (WABAs) through CleverTap as a proxy layer. This integration supports the full conversation lifecycle, from a marketer triggering a campaign to a user replying, the chatbot responding, and events being tracked back in CleverTap.

# Communication Flow

For a full walkthrough of how marketers configure and trigger WhatsApp campaigns in CleverTap, and how the end-to-end conversation flow works from a marketer's perspective, refer to [WhatsApp Chatbot Integration](https://docs.clevertap.com/docs/whatsapp-chatbot-integration) in the CleverTap user documentation.

# Supported APIs

CleverTap exposes the following APIs at the WABA level:

| API                                                                                 | Description                                                                   |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| [Messages API](#messages-api)                                                       | Send template messages to users.                                              |
| [Templates API](#templates-api)                                                     | Create, retrieve, and update message templates.                               |
| [Flows API](#flows-api)                                                             | Create, update, publish, and deprecate WhatsApp Flows.                        |
| [Webhook Notifications](#webhook-notifications)                                     | Receive delivery status updates and user reply events.                        |
| [Upload Events API](https://developer.clevertap.com/docs/upload-events-api)         | Send user interaction events back to CleverTap for tracking and segmentation. |
| [Get User Profiles API](https://developer.clevertap.com/docs/get-user-profiles-api) | Retrieve user profile data to send personalized messages.                     |

# Base URL

Here is an example base URL for this integration in the India region:

```
https://api.clevertap.com/v1/whatsapp/
```

To identify the API endpoint for the region of the account, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

# Headers

For more information about the API headers used while processing API requests, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

All requests to this integration require a `Bearer` token in the `Authorization` header. CleverTap provisions a unique token for each WABA and its associated phone number. Contact the CleverTap account representative to obtain credentials.

```
Authorization: Bearer {{Token}}
```

<Callout icon="📘" theme="info">
  ### API Compatibility

  CleverTap's Proxy APIs are fully compatible with the standard [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api) specification. For valid, correctly formatted requests, the proxy APIs behave identically to their WhatsApp Cloud API counterparts.
</Callout>

# Messages API

The _Messages API_ allows chatbot providers to send WhatsApp template messages to users through CleverTap's proxy layer. The request and response formats are identical to the standard [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api).

## Send Message

Sends a WhatsApp template message to an individual user.

### HTTP Method

POST

### Base URL

```
POST HOST/v1/whatsapp/messages
```

### Query Parameters

| Parameter             | Type    | Required/Optional | Description                                                                                                                          |
| --------------------- | ------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `isMarketingTemplate` | Boolean | Optional          | Set to `true` to route the request through the MmLite endpoint, which is required for Marketing template types. Defaults to `false`. |

### Request Body

The request body follows the same format as the [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api/messages) with no changes. The following fields have CleverTap-specific behavior.

| Field                      | Type   | Required/Optional | Description                                                                                                                                                                          |
| -------------------------- | ------ | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `to`                       | String | Required          | The recipient's phone number in E.164 format.                                                                                                                                        |
| `biz_opaque_callback_data` | String | Optional          | An opaque string passed through to all webhook notifications for this message. Use this to correlate delivery events and user replies back to a specific CleverTap campaign or user. |
| `type`                     | String | Required          | Must be `template` for all messages sent through this integration.                                                                                                                   |

### Sample Request

Here is an example request to the Send Message endpoint, including the headers needed to authenticate the request:

```bash
curl --location 'https://api.clevertap.com/v1/whatsapp/messages' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}' \
--data '{
    "messaging_product": "whatsapp",
    "recipient_type": "individual",
    "to": "9987102608",
    "biz_opaque_callback_data": "abc12345",
    "type": "template",
    "template": {
        "name": "ttytfhgfh",
        "language": {
            "code": "en"
        },
        "components": [
            {
                "type": "body",
                "parameters": []
            },
            {
                "type": "button",
                "index": 0,
                "sub_type": "COPY_CODE",
                "parameters": [
                    {
                        "type": "COUPON_CODE",
                        "coupon_code": "SAMPLECODE"
                    }
                ]
            },
            {
                "type": "button",
                "index": 3,
                "sub_type": "quick_reply"
            }
        ]
    }
}'
```

### Sample Response

On a successful request, the API returns the WhatsApp message ID and acceptance status.

```json
{
    "messaging_product": "whatsapp",
    "contacts": [
        {
            "input": "9987102608",
            "wa_id": "919987102608"
        }
    ],
    "messages": [
        {
            "id": "UUID",
            "message_status": "accepted"
        }
    ]
}
```

<Callout icon="📘" theme="info">
  ### Message ID

  The `id` field in the response is generated by CleverTap, not by Meta.
</Callout>

When Meta returns an error, CleverTap passes it through with Meta's original HTTP status code and body intact. Here is an example Meta error response:

```json
{
    "error": {
        "message": "Unsupported post request. Object with ID 'messages' does not exist, cannot be loaded due to missing permissions, or does not support this operation. Please read the Graph API documentation at https://developers.facebook.com/docs/graph-api",
        "type": "GraphMethodException",
        "code": 100,
        "error_subcode": 33,
        "fbtrace_id": "A3BhRpm5DLU5-xutm2_Um4f"
    }
}
```

## Campaign Message Payload Reference

For details on the payload structure CleverTap sends to the WhatsApp API when a marketer triggers a campaign, refer to [WhatsApp Chatbot Integration](https://docs.clevertap.com/docs/whatsapp-chatbot-integration) in the CleverTap user documentation.

# Templates API

The _Templates API_ allows chatbot providers to retrieve, create, and update WhatsApp message templates for a given WABA through CleverTap's proxy layer. Request and response formats are identical to the standard [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api).

## Get All Templates

Retrieves all message templates associated with the WABA.

### HTTP Method

GET

### Base URL

```
GET HOST/v1/whatsapp/templates
```

### Sample Request

Here is an example request to the Get All Templates endpoint:

```bash
curl --location 'https://api.clevertap.com/v1/whatsapp/templates' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}'
```

### Sample Response

On a successful request, the API returns a paginated list of all message templates associated with the WABA.

```json
{
    "data": [
        {
            "name": "audio_with_transcript",
            "parameter_format": "POSITIONAL",
            "components": [
                {
                    "type": "HEADER",
                    "format": "VIDEO",
                    "example": {
                        "header_handle": [
                            "https://scontent.whatsapp.net/..."
                        ]
                    }
                },
                {
                    "type": "BODY",
                    "text": "LA LA LA LA LA LA"
                }
            ],
            "language": "en",
            "status": "APPROVED",
            "category": "MARKETING",
            "id": "1823697188229282"
        },
        {
            "name": "uc_tempalte",
            "parameter_format": "POSITIONAL",
            "components": [
                {
                    "type": "BODY",
                    "text": "Reminder: Our technician will visit your location on {{1}} at {{2}} for your broadband installation. Please be available.",
                    "example": {
                        "body_text": [
                            ["2025-12-31", "10:00 AM"]
                        ]
                    }
                }
            ],
            "language": "en_US",
            "status": "APPROVED",
            "category": "UTILITY",
            "library_template_name": "appointment_reminder",
            "id": "2689220451415875"
        }
    ],
    "paging": {
        "cursors": {
            "before": "QVFIU2V0N...",
            "after": "QVFIU1pwME..."
        }
    }
}
```

## Get Single Template

Retrieves the details of a specific message template by its ID.

### HTTP Method

GET

### Base URL

```
GET HOST/v1/whatsapp/templates/<TEMPLATE_ID>
```

### Sample Request

Here is an example request to the Get Single Template endpoint:

```bash
curl --location --request GET \
'https://api.clevertap.com/v1/whatsapp/templates/564431566696274' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}'
```

### Sample Response

On a successful request, the API returns the full details of the specified template, including its components, language, status, and category.

```json
{
    "name": "clevertap_monsoon_sale_template_3",
    "previous_category": "UTILITY",
    "parameter_format": "POSITIONAL",
    "components": [
        {
            "type": "BODY",
            "text": "Hey {{1}}, our monsoon sale is live. Check out our website now!",
            "example": {
                "body_text": [["Jay"]]
            }
        }
    ],
    "language": "en_US",
    "status": "APPROVED",
    "category": "MARKETING",
    "id": "564431566696274"
}
```

## Create Template

Creates a new WhatsApp message template for the WABA. The request body follows the same format as the [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/messages) with no changes.

### HTTP Method

POST

### Base URL

```
POST HOST/v1/whatsapp/templates
```

### Sample Request

Here is an example request to create a new message template:

```bash
curl --location 'https://api.clevertap.com/v1/whatsapp/templates' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}' \
--data '{
    "name": "clevertap_monsoon_sale_template_3",
    "previous_category": "UTILITY",
    "language": "en",
    "parameter_format": "POSITIONAL",
    "components": [
        {
            "type": "BODY",
            "text": "Hey {{1}}, our monsoon sale is live. Check out our website now!",
            "example": {
                "body_text": [["Jay"]]
            }
        }
    ],
    "category": "UTILITY"
}'
```

### Sample Response

On a successful request, the API returns the ID, status, and category of the newly created template.

```json
{
    "id": "564431566696274",
    "status": "PENDING",
    "category": "MARKETING"
}
```

<Callout icon="📘" theme="info">
  ### Template Status

  Newly created templates are assigned a `PENDING` status. Meta reviews the template before it transitions to `APPROVED` or `REJECTED`. For more information, refer to [WhatsApp Message Templates](https://docs.clevertap.com/docs/whatsapp-message-templates).
</Callout>

When Meta returns an error, CleverTap passes it through with Meta's original HTTP status code and body intact. Here is an example Meta error response:

```json
{
    "error": {
        "message": "Invalid parameter",
        "type": "OAuthException",
        "code": 100,
        "error_subcode": 2388024,
        "is_transient": false,
        "error_user_title": "Content in this language already exists",
        "error_user_msg": "There is already English content for this template. You can create a new template and try again.",
        "fbtrace_id": "ArsXL02HqZYKW8Udf2ue1gN"
    }
}
```

## Update Template

Updates an existing WhatsApp message template. At least one of `components` or `category` must be provided in the request body. The request body follows the same format as the [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/messages) with no changes.

### HTTP Method

PUT

### Base URL

```
PUT HOST/v1/whatsapp/templates/<TEMPLATE_ID>
```

### Sample Request

Here is an example request to update an existing message template:

```bash
curl --location --request PUT \
'https://api.clevertap.com/v1/whatsapp/templates/564431566696274' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}' \
--data '{
    "name": "clevertap_monsoon_sale_template_3",
    "language": "en_US",
    "parameter_format": "POSITIONAL",
    "components": [
        {
            "type": "BODY",
            "text": "Hey {{1}}, our monsoon sale is live. Check out our website now!, Hurry up!",
            "example": {
                "body_text": [["Jay"]]
            }
        }
    ],
    "category": "MARKETING"
}'
```

### Sample Response

On a successful request, the API returns a confirmation with the updated template ID, name, and category.

```json
{
    "success": true,
    "id": "564431566696274",
    "name": "clevertap_monsoon_sale_template_3",
    "category": "MARKETING"
}
```

When Meta returns an error, CleverTap passes it through with Meta's original HTTP status code and body intact. Here is an example Meta error response:

```json
{
    "error": {
        "message": "Invalid parameter",
        "type": "OAuthException",
        "code": 100,
        "error_subcode": 3835015,
        "is_transient": false,
        "error_user_title": "Message edit missing components or category field",
        "error_user_msg": "When editing a message template, both components and category field cannot be empty. Please provide a new value for components or category when editing the message template.",
        "fbtrace_id": "A6XraVA74qp_nktT7KxETAF"
    }
}
```

# Flows API

The _Flows API_ allows chatbot providers to create and manage WhatsApp Flows for a given WABA through CleverTap's proxy layer. Flows enable structured, interactive conversational experiences within WhatsApp. Request and response formats are identical to the standard [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/flows).

## Understand Flow States

Before working with flows, it is important to understand the valid flow states and the transitions between them.

| State        | Description                                      | Next Possible State |
| ------------ | ------------------------------------------------ | ------------------- |
| `DRAFT`      | Flow has been created but not yet published.     | `PUBLISHED`         |
| `PUBLISHED`  | Flow is live and can be sent to users.           | `DEPRECATED`        |
| `DEPRECATED` | Flow has been retired and can no longer be sent. | None                |

## Get Single Flow

Retrieves the details of a specific WhatsApp Flow by its ID.

### HTTP Method

GET

### Base URL

```
GET HOST/v1/whatsapp/flows/<FLOW_ID>
```

### Sample Request

Here is an example request to the Get Single Flow endpoint:

```bash
curl --location 'https://api.clevertap.com/v1/whatsapp/flows/679234614457259' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Token}}'
```

### Sample Response

On a successful request, the API returns the name, status, categories, and validation errors for the specified flow.

```json
{
    "name": "test_flow_jay_2_new",
    "status": "DRAFT",
    "categories": ["OTHER"],
    "validation_errors": [],
    "id": "679234614457259"
}
```

When Meta returns an error, CleverTap passes it through with Meta's original HTTP status code and body intact. Here is an example Meta error response:

```json
{
    "error": {
        "message": "(#100) Invalid parameter",
        "type": "OAuthException",
        "code": 100,
        "fbtrace_id": "A6LyAvYlMP08MS1Xm9i_pFN"
    }
}
```

## Create Flow

Creates a new WhatsApp Flow for the WABA.

### HTTP Method

POST

### Base URL

```
POST HOST/v1/whatsapp/flows
```

### Request Body

The request uses `multipart/form-data` and follows the same format as the [WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/flows/reference/flowsapi).

| Key            | Type | Required/Optional | Description                                                 |
| -------------- | ---- | ----------------- | ----------------------------------------------------------- |
| `name`         | Text | Required          | The name of the flow.                                       |
| `categories`   | Text | Required          | A JSON array of category strings. For example, `["OTHER"]`. |
| `endpoint_uri` | Text | Optional          | The URI of the endpoint that handles flow data exchange.    |

### Sample Request

Here is an example request to create a new WhatsApp Flow:

```bash
curl --location 'https://api.clevertap.com/v1/whatsapp/flows' \
--header 'Authorization: Bearer {{Token}}' \
--form 'name="test_flow_jay_21010"' \
--form 'categories="[\"OTHER\"]"' \
--form 'endpoint_uri="https://your-endpoint.example.com"'
```

### Sample Response

On a successful request, the API returns the ID of the newly created flow and confirms there are no validation errors.

```json
{
    "id": "1056964379846228",
    "success": true,
    "validation_errors": []
}
```

## Update Flow Metadata

Updates the metadata of an existing WhatsApp Flow, such as its name, categories, or endpoint URI.

### HTTP Method

PUT

### Base URL

```
PUT HOST/v1/whatsapp/flows/<FLOW_ID>
```

### Request Body

The request uses `multipart/form-data`.

| Key          | Type | Required/Optional | Description                                                       |
| ------------ | ---- | ----------------- | ----------------------------------------------------------------- |
| `name`       | Text | Optional          | Updated name for the flow.                                        |
| `categories` | Text | Optional          | Updated JSON array of category strings. For example, `["OTHER"]`. |

### Sample Request

Here is an example request to update the metadata of an existing flow:

```bash
curl --location --request PUT \
'https://api.clevertap.com/v1/whatsapp/flows/1056964379846228' \
--header 'Authorization: Bearer {{Token}}' \
--form 'name="test_flow_jay_21011"' \
--form 'categories="[\"OTHER\"]"' \
--form 'endpoint_uri="https://your-endpoint.example.com"'
```

### Sample Response

On a successful request, the API returns a confirmation with an empty `validation_errors` array.

```json
{
    "success": true,
    "validation_errors": []
}
```

## Update Flow JSON

Uploads or replaces the Flow JSON asset for an existing WhatsApp Flow. Use this endpoint to define or update the interactive screens and logic that make up a flow.

### HTTP Method

PUT

### Base URL

```
PUT HOST/v1/whatsapp/flows/<FLOW_ID>/assets
```

### Request Body

The request uses `multipart/form-data`.

| Key          | Type | Required/Optional | Description                     |
| ------------ | ---- | ----------------- | ------------------------------- |
| `file`       | File | Required          | The `flow.json` file to upload. |
| `name`       | Text | Required          | Must be `flow.json`.            |
| `type`       | Text | Required          | Must be `application/json`.     |
| `asset_type` | Text | Required          | Must be `FLOW_JSON`.            |

### Sample Request

Here is an example request to upload a Flow JSON asset:

```bash
curl --location --request PUT \
'https://api.clevertap.com/v1/whatsapp/flows/1056964379846228/assets' \
--header 'Authorization: Bearer {{Token}}' \
--form 'file=@"flow.json"' \
--form 'name="flow.json"' \
--form 'type="application/json"' \
--form 'asset_type="FLOW_JSON"'
```

### Sample Response

On a successful request, the API returns a confirmation. If the uploaded Flow JSON contains structural issues, they are listed in the `validation_errors` array.

```json
{
    "success": true,
    "validation_errors": []
}
```

<Callout icon="📘" theme="info">
  ### Validation Errors

  A non-empty `validation_errors` array does not prevent the upload but indicates changes are required before the flow can be published. For more information on building valid flow JSON, refer to [WhatsApp Flows](https://docs.clevertap.com/docs/whatsapp-flows).
</Callout>

## Publish Flow

Publishes a WhatsApp Flow, making it available to send to users.

<Callout icon="⚠️" theme="warn">
  ### Before Publishing

  Ensure the Flow endpoint is live and has passed a health check. Publishing will fail if the endpoint is unavailable. Resolve any `validation_errors` from the [Update Flow JSON](#update-flow-json) step before calling this endpoint.
</Callout>

### HTTP Method

POST

### Base URL

```
POST HOST/v1/whatsapp/flows/<FLOW_ID>/publish
```

### Sample Request

Here is an example request to publish a WhatsApp Flow:

```bash
curl --location --request POST \
'https://api.clevertap.com/v1/whatsapp/flows/1056964379846228/publish' \
--header 'Authorization: Bearer {{Token}}' \
--header 'Content-Type: application/json'
```

### Sample Response

On a successful request, the API returns a confirmation that the flow has been published.

```json
{
    "success": true
}
```

When Meta returns an error, CleverTap passes it through with Meta's original HTTP status code and body intact. Here is an example Meta error response:

```json
{
    "error": {
        "message": "Publishing attempt failed",
        "type": "OAuthException",
        "code": 139002,
        "error_subcode": 4233014,
        "is_transient": false,
        "error_user_title": "Endpoint not available",
        "error_user_msg": "You need to verify that the endpoint is available and that you've implemented a health check before publishing.",
        "fbtrace_id": "AegeqSzIT7p-ZJVqKQ8sv7y"
    }
}
```

## Deprecate Flow

Deprecates a published WhatsApp Flow. Once deprecated, the flow can no longer be triggered or sent to users.

<Callout icon="📘" theme="info">
  ### Note

  Only flows that have been previously published can be deprecated. Attempting to deprecate a flow in `DRAFT` status will return an error from Meta.
</Callout>

### HTTP Method

POST

### Base URL

```
POST HOST/v1/whatsapp/flows/<FLOW_ID>/deprecate
```

### Sample Request

Here is an example request to deprecate a published flow:

```bash
curl --location --request POST \
'https://api.clevertap.com/v1/whatsapp/flows/1056964379846228/deprecate' \
--header 'Authorization: Bearer {{Token}}' \
--header 'Content-Type: application/json'
```

### Sample Response

On a successful request, the API returns a confirmation that the flow has been deprecated.

```json
{
    "success": true
}
```

When Meta returns an error, CleverTap passes it through with Meta's original HTTP status code and body intact. Here is an example Meta error response:

```json
{
    "error": {
        "message": "Deprecating attempt failed",
        "type": "OAuthException",
        "code": 139003,
        "error_subcode": 4016007,
        "is_transient": false,
        "error_user_title": "Can't deprecate flow",
        "error_user_msg": "Flow was never published, it cannot be deprecated",
        "fbtrace_id": "AIFZwmPtBmLKOd-AAsskw9_"
    }
}
```

# Webhook Notifications

CleverTap forwards WhatsApp webhook events to the chatbot provider's configured endpoint. These events cover message delivery status updates and user replies, allowing chatbot providers to respond dynamically to user interactions.

<Callout icon="📘" theme="info">
  ### Security

  For security reasons, CleverTap forwards only the `config_id` in webhook payloads instead of the full WhatsApp Business Account (WABA) ID and `fromNumberId`. The `config_id` is a unique identifier scoped to the WABA and its associated phone number.
</Callout>

<Callout icon="📘" theme="info">
  ### Payload Format

  CleverTap forwards all webhook events in the simplified array format described in this section. The legacy payload format that wrapped events inside `object`, `entry`, and `changes` fields is deprecated and no longer in use.
</Callout>

## Notification Event Types

The following events are forwarded to the chatbot provider's configured webhook endpoint. Each event type includes a sample payload.

### Notification Sent

Triggered when a message is accepted and sent by the WhatsApp platform.

```json
[
    {
        "value": {
            "messaging_product": "whatsapp",
            "config_id": "12345678901",
            "statuses": [
                {
                    "id": "wamid.HBgMOTE3...",
                    "status": "sent",
                    "timestamp": "1756807289",
                    "recipient_id": "917697854932",
                    "biz_opaque_callback_data": "5170044|1756807149|20250902|5170044|0",
                    "conversation": {
                        "id": "e3c64e1b75b13c6e3e2be0909f7ab34d",
                        "expiration_timestamp": "1756807289",
                        "origin": {
                            "type": "utility"
                        }
                    },
                    "pricing": {
                        "billable": true,
                        "pricing_model": "PMP",
                        "category": "utility",
                        "type": "regular"
                    }
                }
            ]
        },
        "field": "messages"
    }
]
```

### Notification Delivered

Triggered when a message is successfully delivered to the recipient's device.

```json
[
    {
        "value": {
            "messaging_product": "whatsapp",
            "config_id": "12345678901",
            "statuses": [
                {
                    "id": "wamid.HBgMOTE5...",
                    "status": "delivered",
                    "timestamp": "1756788065",
                    "recipient_id": "919987102608",
                    "biz_opaque_callback_data": "269356|1756788044|20250902|269356|0",
                    "conversation": {
                        "id": "c8ab9dace7e47694041eb1ae93ae454c",
                        "origin": {
                            "type": "utility"
                        }
                    },
                    "pricing": {
                        "billable": true,
                        "pricing_model": "PMP",
                        "category": "utility",
                        "type": "regular"
                    }
                }
            ]
        },
        "field": "messages"
    }
]
```

### Notification Failed

Triggered when a message fails to deliver. The `errors` array contains the failure reason and error code from Meta.

```json
[
    {
        "value": {
            "messaging_product": "whatsapp",
            "config_id": "12345678901",
            "statuses": [
                {
                    "id": "wamid.HBgMOTE4...",
                    "status": "failed",
                    "timestamp": "1756807288",
                    "recipient_id": "918953146651",
                    "biz_opaque_callback_data": "5170039|1756807149|20250902|5170039|0",
                    "errors": [
                        {
                            "code": 131026,
                            "title": "Message undeliverable",
                            "message": "Message undeliverable",
                            "error_data": {
                                "details": "Message Undeliverable."
                            }
                        }
                    ]
                }
            ]
        },
        "field": "messages"
    }
]
```

### User Reply: No Context

Triggered when a user sends a free-text reply that is not in response to a specific message. No `context` field is present in the payload.

```json
[
    {
        "value": {
            "messaging_product": "whatsapp",
            "config_id": "12345678901",
            "contacts": [
                {
                    "profile": {
                        "name": "Jay Kotak"
                    },
                    "wa_id": "919987102608"
                }
            ],
            "messages": [
                {
                    "from": "919987102608",
                    "id": "wamid.HBgMOTE5...",
                    "timestamp": "1750847339",
                    "text": {
                        "body": "hi"
                    },
                    "type": "text"
                }
            ]
        },
        "field": "messages"
    }
]
```

### Quick Reply

Triggered when a user taps a quick reply button on a template message. The `button.payload` contains the payload value configured on the button, and `button.text` contains the button label displayed to the user.

```json
[
    {
        "value": {
            "messaging_product": "whatsapp",
            "config_id": "12345678901",
            "contacts": [
                {
                    "profile": {
                        "name": "Jay Kotak"
                    },
                    "wa_id": "919987102608"
                }
            ],
            "messages": [
                {
                    "context": {
                        "from": "919819134112",
                        "id": "wamid.HBgMOTE5..."
                    },
                    "from": "919987102608",
                    "id": "wamid.HBgMOTE5...",
                    "timestamp": "1760338853",
                    "type": "button",
                    "button": {
                        "payload": "9987102608",
                        "text": "Track order"
                    }
                }
            ]
        },
        "field": "messages"
    }
]
```

### Swipe-to-Reply

Triggered when a user swipes left on a message to reply. The `context` field references the original message the user is replying to.

```json
[
    {
        "value": {
            "messaging_product": "whatsapp",
            "config_id": "12345678901",
            "contacts": [
                {
                    "profile": {
                        "name": "Jay Kotak"
                    },
                    "wa_id": "919987102608"
                }
            ],
            "messages": [
                {
                    "context": {
                        "from": "919819134112",
                        "id": "wamid.HBgMOTE5..."
                    },
                    "from": "919987102608",
                    "id": "wamid.HBgMOTE5...",
                    "timestamp": "1760339043",
                    "text": {
                        "body": "Some context"
                    },
                    "type": "text"
                }
            ]
        },
        "field": "messages"
    }
]
```

## Payload Field Reference

The following fields are common across all incoming webhook event payloads.

| Field                      | Type   | Description                                                                                                                                                  |
| -------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `messaging_product`        | String | Always `whatsapp`.                                                                                                                                           |
| `config_id`                | String | Unique identifier for the WABA and phone number configuration in CleverTap.                                                                                  |
| `statuses`                 | Array  | Present on delivery status events (`sent`, `delivered`, `failed`). Contains status details for each message.                                                 |
| `contacts`                 | Array  | Present on reply events. Contains the sender's profile name and WhatsApp ID.                                                                                 |
| `messages`                 | Array  | Present on reply events. Contains the message content, type, and optional `context`.                                                                         |
| `biz_opaque_callback_data` | String | The opaque callback string is attached to the original outbound message. Used to correlate replies and delivery events back to a CleverTap campaign or user. |
| `context`                  | Object | Present on Quick Reply and Swipe-to-Reply events. Contains the `from` number and `id` of the message being replied to.                                       |

## Upload Events to CleverTap

After processing a webhook event, chatbot providers can upload user interaction events to CleverTap to enrich user profiles and power downstream segmentation and campaigns.

In addition to standard identity types (`identity`, `FBID`, `GPID`, `objectId`), CleverTap supports `biz_opaque_callback_data` as a user identifier for WhatsApp interactions. For full documentation, refer to [Upload Events API](https://developer.clevertap.com/docs/upload-events-api).

### Sample Request

Here is an example request to upload a user interaction event using `biz_opaque_callback_data` as the identifier:

```bash
curl --location 'https://in1.api.clevertap.com/1/upload' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json; charset=utf-8' \
--data '{
    "d": [
        {
            "biz_opaque_callback_data": "269356|1756788044|20250902|269356|0",
            "ts": 1468308340,
            "type": "event",
            "evtName": "Product viewed",
            "evtData": {
                "Product name": "Casio Chronograph Watch",
                "Category": "Mens Watch",
                "Price": 59.99,
                "Currency": "USD"
            }
        }
    ]
}'
```

# Errors

To understand the errors applicable to this integration, refer to [API Errors](https://developer.clevertap.com/docs/api-errors).

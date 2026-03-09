---
title: Notify API
excerpt: >-
  Learn how to send real-time, personalized transactional email campaigns to
  users via the Notify API.
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

The Notify API enables you to send a user real-time, personalized transactional messages through a predefined Notify setup created on the CleverTap dashboard. For more information about the feature and how to set up a notify campaign on the CleverTap dashboard, refer to [Notify User Guide](https://docs.clevertap.com/docs/notify).

<Callout icon="📘" theme="info">
  **Private Beta**

  Notify is currently released in Private Beta for the Email channel with SendGrid as the email provider. To access this feature, contact your Customer Success Manager.
</Callout>

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/notify](https://in1.api.clevertap.com/v1/notify)

# Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# HTTP Method

POST

# Headers

To authenticate and process your Notify API request, you must include the following headers in every API request:

| Header                   | Value              | Required | Description                                                                                                                                                                                                                                         |
| ------------------------ | ------------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Content-Type`           | `application/json` | Yes      | Payload format                                                                                                                                                                                                                                      |
| `X-CleverTap-Account-Id` | `Your Account ID`  | Yes      | Your CleverTap Account ID. You can find this information on the _Settings_ > _Projects_ page.                                                                                                                                                       |
| `X-CleverTap-Passcode`   | `Your Passcode`    | Yes      | Your CleverTap Passcode. You can find this information on the _Settings_ > _Projects_ page. For more information about API headers used while processing API requests. For more information, refer to [Headers](doc:common-api-components#headers). |

# Body Parameters

The request body includes the following parameters:

| Parameter      | Type   | Description                                                                                                                                                                                                   | Required                                                                                                                                                                                                                 |
| :------------- | :----- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `identity`     | string | Identity of the user profile to whom the message must be sent. Use this when the recipient is a known user in CleverTap.                                                                                      | Yes, if `recipient` is not provided.                                                                                                                                                                                     |
| `notifyId`     | string | Unique ID of the Notify setup configured on the CleverTap dashboard. For more information, refer to [Create Notify Campaign](https://docs.clevertap.com/docs/notify#create-notify-campaign).                  | Yes                                                                                                                                                                                                                      |
| `recipient`    | object | Object used to specify the recipient's email address directly.                                                                                                                                                | Yes. This parameter is mandatory if:<ul><li> User profile does not exist in CleverTap </li> <li>`identity` information is missing for the user profile</li><li> You want to override the  existing `identity`.</li></ul> |
| `notifyFields` | object | Key-value pairs used to personalize the message content. Each key must match the variable names defined in the message template. Pass an empty object (`{}`) if the message does not include personalization. | Required only if the message contains personalization variables.                                                                                                                                                         |

# User Identification and Attribution

CleverTap determines how to send Notify campaigns based on the presence of `identity` and `recipient` parameters in the API request. The logic below shows how CleverTap handles user resolution and message delivery based on the request payload.

To deliver a Notify message, your API request must include at least one of the following fields:

* `identity`: A unique identifier linked to a CleverTap user profile
* `recipient`: The email address where the message should be sent

## User Resolution Logic for Notify Messages

The table below outlines how CleverTap handles message delivery based on different combinations of the `identity` and `recipient` parameters:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameters
      </th>

      <th>
        Description
      </th>

      <th>
        Example Payload
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        When both

        `identity`

        and

        `recipient`

        are present
      </td>

      <td>
        <li>Sends the email to the email ID specified in the `recipient` field.</li>
        <li>Used when the user profile exists on the dashboard but does not have an email ID, or the message must be sent to an alternate email address.</li>
      </td>

      <td>
        `{     "identity": "user_123",     "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",     "recipient": {       "email": "[abc@example.com](mailto:abc@example.com) "     },     "notifyFields": {}   }`
      </td>
    </tr>

    <tr>
      <td>
        When

        `identity`

        is present and

        `recipient`

        is not present
      </td>

      <td>
        Sends an email to the email ID of the profile whose

        `identity`

        is present.
      </td>

      <td>
        `{     "identity": "user_123",     "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",     "notifyFields": {}   }`
      </td>
    </tr>

    <tr>
      <td>
        When

        `identity`

        is not present but

        `recipient`

        is present
      </td>

      <td>
        <ul><li>Sends an email to the email ID mentioned in the `recipient` field.</li>
        <li>Used when the recipient profile does not exist on the dashboard. The message is still sent, but delivery or engagement metrics are not tracked for such profiles.</li></ul>
      </td>

      <td>
        `{     "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",     "recipient": {       "email": "[new_user@example.com]"     },     "notifyFields": {}   }`
      </td>
    </tr>
  </tbody>
</Table>

# Sample Request

To trigger a Notify campaign via API, include the following parameters in your request body: user `identity` or `recipient` details, `notifyId`, and personalization data through `notifyFields`(optional).
When setting up a Notify campaign in the CleverTap dashboard, you can personalize messages using template variables (placeholders) such as `{{Notify.order_id}}`. The placeholders are dynamically replaced with user-specific values from the API payload. For more information, refer to [Create Notify Campaign](https://docs.clevertap.com/docs/notify#create-notify-campaign).

For example, to trigger an order confirmation Notify campaign for a specific user profile (with identity `123`) with the order ID, the API request may look like this:

```curl
curl -X POST https://api.clevertap.com/v1/notify \
  -H "Content-Type: application/json" \
  -H "X-CleverTap-Account-Id: YOUR_ACCOUNT_ID" \
  -H "X-CleverTap-Passcode: YOUR_PASSCODE" \
  -d '{
    "identity": "123",
    "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",
    "recipient": { "email": "abc@gmail.com" },
    "notifyFields": { "order_id": "A1234" }
  }
```

# Sample Response

If the request is valid, the API responds with a success message and a unique request ID.

```json
{
  "status": "success",         // Status of request
  "requestId": "429374...",    // Unique request identifier
  "message": "Added to queue"  // Delivery status message
}
```

# Error Codes

In case of an error in the Notify delivery, the following error codes indicate the nature of the error encountered:

| **Error Code** | **HTTP Status Code** | **Error Name**             | **Description**                                                                                                                                                                                              | **Response Payload**                                                                                    |
| -------------- | -------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| 101            | 403                  | `FEATURE_NOT_ENABLED`      | Notify feature is not enabled for this account.                                                                                                                                                              | `{"status": "fail", "error": "Notify feature is not enabled for this account", "code": 101}`            |
| 102            | 400                  | `INVALID_NOTIFY_ID`        | The Notify ID provided in the request is invalid.                                                                                                                                                            | `{"status": "fail", "error": "Invalid Notify ID", "code": 102}`                                         |
| 103            | 400                  | `INACTIVE_NOTIFY_CAMPAIGN` | The Notify ID refers to a campaign that is not active or has been stopped.                                                                                                                                   | `{"status": "fail", "error": "Inactive Notify ID", "code": 103}`                                        |
| 104            | 400                  | `USER_NOT_FOUND`           | No matching user profile was found for the identity provided in the request.                                                                                                                                 | `{"status": "fail", "error": "Profile not found", "code": 104}`                                         |
| 105            | 413                  | `PAYLOAD_LENGTH_EXCEEDED`  | The payload size exceeds the permissible limit of 100KB.                                                                                                                                                     | `{"status": "fail", "error": "The payload length exceeds the permissible limit of 100KB", "code": 105}` |
| 106            | 400                  | `JSON_PROCESSING_ERROR`    | The request body contains invalid or malformed JSON.                                                                                                                                                         | `{"status": "fail", "error": "Invalid JSON payload", "code": 106}`                                      |
| 107            | 400                  | `MISSING_REACHABLE_FIELDS` | At least one of `identity` or `recipient` must be provided in the request.                                                                                                                                   | `{"status": "fail", "error": "Either identity or recipients field must be provided ", "code": 107}`     |
| 108            | 400                  | `INVALID_REQUEST`          | The request fails to meet one or more validation constraints. This can occur if required parameters are missing, or fields exceed allowed limits (for example, Notify ID > 36 bytes, phone > 20 characters). | `{"status": "fail", "error": "Invalid request", "code": 108}`                                           |
| 109            | 500                  | `UNEXPECTED_ERROR`         | An unexpected server error occurred. Try again or contact support if needed.                                                                                                                                 | `{"status": "fail", "error": "Something went wrong!", "code": 109}`                                     |

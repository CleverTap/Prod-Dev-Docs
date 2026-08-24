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

The Notify API enables you to send a user real-time, personalized transactional messages through a predefined Notify setup created on the CleverTap dashboard. For more information about the feature and how to set up a notify campaign on the CleverTap dashboard, refer to [Notify User Guide](doc:notify).

<Callout icon="📘" theme="info">
  ### Private Beta

  Notify is currently in Private Beta for the Email and SMS channels. The feature is currently available for the following providers:

  - Email: SendGrid and Infobip (with the Advanced Email Add-on)

  - SMS: Generic and Infobip (with the SMS Direct addon)

  To enable this feature, contact your Customer Success Manager.
</Callout>

# Base URL

Here is an example base URL from the account in the India region:

`https://in1.api.clevertap.com/v1/notify`

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

| Parameter                 | Type             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                               | Required                                                                                                                                                                                                        |
| :------------------------ | :--------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `identity`                | string           | Identity of the user profile to whom the message must be sent. Use this when the recipient is a known user in CleverTap.                                                                                                                                                                                                                                                                                                                                  | Yes, if `recipient` is not provided.                                                                                                                                                                            |
| `notifyId`                | string           | Unique ID of the Notify setup configured on the CleverTap dashboard. For more information, refer to [Create Notify Campaign](https://staging.docs.user.clevertap.net/docs/notify#create-notify-campaign).                                                                                                                                                                                                                                                 | Yes                                                                                                                                                                                                             |
| `recipient`               | object           | Object used to specify the recipient's email address or phone number directly.                                                                                                                                                                                                                                                                                                                                                                            | Yes. This parameter is mandatory if:<li> User profile does not exist in CleverTap </li> <li>`identity` information is missing for the user profile</li><li> You want to override the  existing `identity`.</li> |
| `recipient.email`         | string           | Email address of the recipient. Required when sending to the Email channel without an identity, or to override the email address on the profile.                                                                                                                                                                                                                                                                                                          | Conditional                                                                                                                                                                                                     |
| `recipient.phone`         | string           | Phone number of the recipient in E.164 format (for example, `+11234567890`). Required when sending to the SMS channel without an identity, or to override the phone number on the profile.                                                                                                                                                                                                                                                                | Conditional                                                                                                                                                                                                     |
| `notifyFields`            | object           | Key-value pairs used to personalize the message content. Each key must match the variable names defined in the message template. Pass an empty object (`{}`) if the message does not include personalization.                                                                                                                                                                                                                                             | Required only if the message contains personalization variables.                                                                                                                                                |
| `attachments`             | array of objects | List of file attachments to include in the email. Max 1 attachment per request. Max file size: \~1 MB.                                                                                                                                                                                                                                                                                                                                                    | No                                                                                                                                                                                                              |
| `attachments.fileName`    | string           | Name of the file including extension (for example, `invoice.pdf`). Max 255 characters.                                                                                                                                                                                                                                                                                                                                                                    | Yes                                                                                                                                                                                                             |
| `attachments.contentType` | string           | MIME type of the file. It must match the file extension. <br />Supported file formats and MIME types:	<ul><li>PDF - application/pdf</li><li>DOC - application/msword</li><li>DOCX - application/vnd.openxmlformats-officedocument.wordprocessingml.document</li><li>ICS - text/calendar</li><li>EML - message/rfc822</li><li>PNG - image/png</li><li>JPEG - image/jpeg</li><li>JPG - image/jpeg</li><li>MP4 -video/mp4</li><li>MP3 - audio/mpeg</li></ul> | Yes                                                                                                                                                                                                             |
| `attachments.data`        | string           | Base64-encoded content of the file.                                                                                                                                                                                                                                                                                                                                                                                                                       | Yes                                                                                                                                                                                                             |

# User Identification and Attribution

CleverTap determines how to send Notify campaigns based on the presence of `identity` and `recipient` parameters in the API request. The logic below shows how CleverTap handles user resolution and message delivery based on the request payload.

To deliver a Notify message, your API request must include at least one of the following fields:

- `identity`: A unique identifier linked to a CleverTap user profile
- `recipient`: The email address or phone number where the message should be sent

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
        <ul>
          <li>Sends the email to the email ID or phone number specified in the `recipient` field.</li>
          <li>Used when the user profile exists on the dashboard but does not have an email ID or a phone number, or the message must be sent to an alternate email address or a phone number.</li>
        </ul>
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
        Sends an email or SMS to the email ID or phone number of the profile whose

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
        <ul>
          <li>Sends an email or a SMS to the email ID or a phone number mentioned in the `recipient` field.</li>
          <li>Used when the recipient profile does not exist on the dashboard. The message is still sent, but delivery or engagement metrics are not tracked for such profiles.</li>
        </ul>
      </td>

      <td>
        `{     "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",     "recipient": {       "email": "[new_user@example.com]"     },     "notifyFields": {}   }`
      </td>
    </tr>
  </tbody>
</Table>

# Sample Request

To trigger a Notify campaign via API, include the following parameters in your request body: user `identity` or `recipient` details, `notifyId`, and personalization data through `notifyFields`(optional).
When setting up a Notify campaign in the CleverTap dashboard, you can personalize messages using template variables (placeholders) such as `{{Notify.order_id}}`. The placeholders are dynamically replaced with user-specific values from the API payload. For more information, refer to [Create Notify Campaign](https://staging.docs.user.clevertap.net/docs/notify#create-notify-campaign).

For example, to trigger an order confirmation, a Notify campaign for a specific user profile (with identity `123`) with the order ID, the API request may look like this:

```curl Email - Without Attachment
curl --location 'https://api.clevertap.com/v1/notify' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_PASSCODE' \
--data-raw '{
    "identity": "123",
    "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",
    "recipient": {
        "email": "abc@gmail.com"
    },
    "notifyFields": {
        "order_id": "A1234"
    }
}'
```
```curl Email - With Attachment
curl --location 'https://api.clevertap.com/v1/notify' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_PASSCODE' \
--data-raw '{
    "identity": "123",
    "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",
    "recipient": { "email": "abc@example.com" },
    "notifyFields": { "order_id": "A1234" },
    "attachments": [
        {
            "fileName": "invoice.pdf",
            "contentType": "application/pdf",
            "data": "<base64-encoded-content>"
        }
    ]
}
```
```text SMS
curl --location 'https://api.clevertap.com/v1/notify' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_PASSCODE' \
--data-raw '{
    "identity": "123",
    "notifyId": "4d06dacc-b52a-4e72-9193-b58c78d5840f",
    "recipient": {
        "phone": "+919876543210",
    },
    "notifyFields": {
        "order_id": "A1234"
    }
}'
```

<Callout icon="📘" theme="info">
  ### Attachments - Requirements and Constraints

  - Maximum attachment size: 1 MB.
  - Maximum filename length: 255 characters.
  - Supported file formats: PDF, DOC, DOCX, ICS, EML, PNG, JPEG, JPG, MP4, and MP3.
  - Only supported for the Email channel.
  - Only 1 attachment is allowed per request.
  - Only supported when your Notify setup uses SendGrid or Infobip (with the Advanced Email Add-on) as the email provider.
  - Requires attachments to be enabled for your account. To enable this feature, contact your Customer Success Manager.
</Callout>

# Sample Response

If the request is valid, the API responds with a success message and a unique request ID. The following is the sample response:

```json
{
  "status": "success",
  "requestId": "",
  "message": "Added to queue for processing"
}
```

# Error Codes

In case of an error in the Notify delivery, the following error codes indicate the nature of the error encountered:

| Error Code | HTTP Status Code | Error Name                                | Description                                                                                                                                                                                                                                    | Response Payload                                                                                                                  |
| ---------- | ---------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| 101        | 403              | `FEATURE_NOT_ENABLED`                     | Notify feature is not enabled for this account.                                                                                                                                                                                                | `{"status": "fail", "error": "Notify feature is not enabled for this account", "code": 101}`                                      |
| 102        | 400              | `INVALID_NOTIFY_ID`                       | The Notify ID provided in the request is invalid.                                                                                                                                                                                              | `{"status": "fail", "error": "Invalid Notify ID", "code": 102}`                                                                   |
| 103        | 400              | `INACTIVE_NOTIFY_ID`                      | The Notify ID refers to a campaign that is not active or has been stopped.                                                                                                                                                                     | `{"status": "fail", "error": "Inactive Notify ID", "code": 103}`                                                                  |
| 104        | 400              | `PROFILE_NOT_FOUND`                       | No matching user profile was found for the identity provided in the request.                                                                                                                                                                   | `{"status": "fail", "error": "Profile not found", "code": 104}`                                                                   |
| 105        | 413              | `PAYLOAD_LENGTH_EXCEEDED`                 | The payload size exceeds the permissible limit of 100 KB.                                                                                                                                                                                      | `{"status": "fail", "error": "The payload length exceeds the permissible limit of 100KB", "code": 105}`                           |
| 106        | 400              | `JSON_PROCESSING_ERROR`                   | The request body contains invalid or malformed JSON.                                                                                                                                                                                           | `{"status": "fail", "error": "Invalid JSON payload", "code": 106}`                                                                |
| 107        | 400              | `MISSING_REACHABLE_FIELDS`                | At least one of `userId` or `recipients` must be provided in the request.                                                                                                                                                                      | `{"status": "fail", "error": "Either userId or recipients field must be provided", "code": 107}`                                  |
| 108        | 400              | `INVALID_PAYLOAD`                         | The request fails to meet one or more validation constraints. This can occur if required parameters are missing, or fields exceed allowed limits (for example, Notify ID > 36 bytes, phone number not in E.164 format or exceeding 15 digits). | `{"status": "fail", "error": "Invalid payload", "code": 108}`                                                                     |
| 109        | 500              | `UNEXPECTED_ERROR`                        | An unexpected server error occurred. Try again or contact support if needed.                                                                                                                                                                   | `{"status": "fail", "error": "Something went wrong!", "code": 109}`                                                               |
| 110        | 400              | `ATTACHMENT_COUNT_EXCEEDED`               | The request includes more attachments than allowed. Only 1 attachment is permitted per request.                                                                                                                                                | `{"status": "fail", "error": "Maximum 1 attachment allowed per request", "code": 110}`                                            |
| 111        | 400              | `ATTACHMENT_SIZE_EXCEEDED`                | The attachment exceeds the maximum allowed size of 1 MB.                                                                                                                                                                                       | `{"status": "fail", "error": "Attachment size exceeds maximum limit of 1 MB", "code": 111}`                                       |
| 112        | 400              | `ATTACHMENT_TYPE_NOT_SUPPORTED`           | The attachment file type is not supported.                                                                                                                                                                                                     | `{"status": "fail", "error": "Attachment file type not supported", "code": 112}`                                                  |
| 113        | 400              | `ATTACHMENT_PROVIDER_NOT_SUPPORTED`       | Email attachments are not supported for the account's configured email provider.                                                                                                                                                               | `{"status": "fail", "error": "Email attachments are not supported for this account's email provider configuration", "code": 113}` |
| 114        | 400              | `INVALID_ATTACHMENT_METADATA`             | The attachment metadata is invalid. `filename` and `contentType` are required, and `data` must be valid base64.                                                                                                                                | `{"status": "fail", "error": "Attachment filename and contentType are required, and data must be valid base64", "code": 114}`     |
| 115        | 400              | `ATTACHMENT_SUPPORT_NOT_ENABLED`          | Attachments in Notify are not enabled for this account.                                                                                                                                                                                        | `{"status": "fail", "error": "Attachments in Notify is not enabled for this account", "code": 115}`                               |
| 116        | 413              | `PAYLOAD_LENGTH_EXCEEDED_WITH_ATTACHMENT` | The payload size exceeds the permissible limit when an attachment is included. Ensure the attachment does not exceed 1 MB, and the total payload does not exceed 1.5 MB.                                                                       | `{"status": "fail", "error": "The payload length exceeds the permissible limit", "code": 116}`                                    |

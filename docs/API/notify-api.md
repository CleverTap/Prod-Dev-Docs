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

| Parameter                 | Type             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Required                                                                                                                                                                                                        |
| :------------------------ | :--------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `identity`                | string           | Identity of the user profile to whom the message must be sent. Use this when the recipient is a known user in CleverTap.                                                                                                                                                                                                                                                                                                                                                       | Yes, if `recipient` is not provided.                                                                                                                                                                            |
| `notifyId`                | string           | Unique ID of the Notify setup configured on the CleverTap dashboard. For more information, refer to [Create Notify Campaign](https://staging.docs.user.clevertap.net/docs/notify#create-notify-campaign).                                                                                                                                                                                                                                                                      | Yes                                                                                                                                                                                                             |
| `recipient`               | object           | Object used to specify the recipient's email address or phone number directly.                                                                                                                                                                                                                                                                                                                                                                                                 | Yes. This parameter is mandatory if:<li> User profile does not exist in CleverTap </li> <li>`identity` information is missing for the user profile</li><li> You want to override the  existing `identity`.</li> |
| `recipient.email`         | string           | Email address of the recipient. Required when sending to the Email channel without an identity, or to override the email address on the profile.                                                                                                                                                                                                                                                                                                                               | Conditional                                                                                                                                                                                                     |
| `recipient.phone`         | string           | Phone number of the recipient in E.164 format (for example, `+11234567890`). Required when sending to the SMS channel without an identity, or to override the phone number on the profile.                                                                                                                                                                                                                                                                                     | Conditional                                                                                                                                                                                                     |
| `notifyFields`            | object           | Key-value pairs used to personalize the message content. Each key must match the variable names defined in the message template. Pass an empty object (`{}`) if the message does not include personalization.                                                                                                                                                                                                                                                                  | Required only if the message contains personalization variables.                                                                                                                                                |
| `attachments`             | array of objects | List of file attachments to include in the email. Max 1 attachment per request. Max file size: \~1 MB.                                                                                                                                                                                                                                                                                                                                                                         | `attachments`                                                                                                                                                                                                   |
| `attachments.fileName`    | string           | Name of the file including extension (for example, `invoice.pdf`). Max 255 characters.                                                                                                                                                                                                                                                                                                                                                                                         | `attachments.fileName`                                                                                                                                                                                          |
| `attachments.contentType` | string           | MIME type of the file. It must match the file extension. <br />Supported file formats and MIME types `<ul><li>`PDF - application/pdf`</li><li>`DOC - application/msword`</li><li>`DOCX - application/vnd.openxmlformats-officedocument.wordprocessingml.document`</li><li>`ICS - text/calendar`</li><li>`EML - message/rfc822`</li><li>`PNG - image/png`</li><li>`JPEG - image/jpeg`</li><li>`JPG - image/jpeg`</li><li>`MP4 -video/mp4`</li><li>`MP3 - audio/mpeg`</li></ul>` | `attachments.contentType`                                                                                                                                                                                       |
| `attachments.data`        | string           | Base64-encoded content of the file.                                                                                                                                                                                                                                                                                                                                                                                                                                            | Yes                                                                                                                                                                                                             |

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

<Table>
  <thead>
    <tr>
      <th>
        **Error Code**
      </th>

      <th>
        **HTTP Status Code**
      </th>

      <th>
        **Error Name**
      </th>

      <th>
        **Description**
      </th>

      <th>
        **Response Payload**
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        101
      </td>

      <td>
        403
      </td>

      <td>
        `FEATURE_NOT_ENABLED`
      </td>

      <td>
        Notify feature is not enabled for this account.
      </td>

      <td>
        `{"status": "fail", "error": "Notify feature is not enabled for this account", "code": 101}`
      </td>
    </tr>

    <tr>
      <td>
        102
      </td>

      <td>
        400
      </td>

      <td>
        `INVALID_NOTIFY_ID`
      </td>

      <td>
        The Notify ID provided in the request is invalid.
      </td>

      <td>
        `{"status": "fail", "error": "Invalid Notify ID", "code": 102}`
      </td>
    </tr>

    <tr>
      <td>
        103
      </td>

      <td>
        400
      </td>

      <td>
        `INACTIVE_NOTIFY_CAMPAIGN`
      </td>

      <td>
        The Notify ID refers to a campaign that is not active or has been stopped.
      </td>

      <td>
        `{"status": "fail", "error": "Inactive Notify ID", "code": 103}`
      </td>
    </tr>

    <tr>
      <td>
        104
      </td>

      <td>
        400
      </td>

      <td>
        `USER_NOT_FOUND`
      </td>

      <td>
        No matching user profile was found for the identity provided in the request.
      </td>

      <td>
        `{"status": "fail", "error": "Profile not found", "code": 104}`
      </td>
    </tr>

    <tr>
      <td>
        106
      </td>

      <td>
        400
      </td>

      <td>
        `JSON_PROCESSING_ERROR`
      </td>

      <td>
        The request body contains invalid or malformed JSON.
      </td>

      <td>
        `{"status": "fail", "error": "Invalid JSON payload", "code": 106}`
      </td>
    </tr>

    <tr>
      <td>
        107
      </td>

      <td>
        400
      </td>

      <td>
        `MISSING_REACHABLE_FIELDS`
      </td>

      <td>
        At least one of `identity` or `recipient` must be provided in the request.
      </td>

      <td>
        `{"status": "fail", "error": "Either identity or recipients field must be provided ", "code": 107}`
      </td>
    </tr>

    <tr>
      <td>
        108
      </td>

      <td>
        400
      </td>

      <td>
        `INVALID_REQUEST`
      </td>

      <td>
        The request fails to meet one or more
        validation constraints. This can occur if required parameters are missing,
        or fields exceed allowed limits (for example, Notify ID > 36 bytes, phone
        number not in E.164 format or exceeding 15 digits).
      </td>

      <td>
        `{"status": "fail", "error": "Invalid request", "code": 108}`
      </td>
    </tr>

    <tr>
      <td>
        109
      </td>

      <td>
        500
      </td>

      <td>
        `UNEXPECTED_ERROR`
      </td>

      <td>
        An unexpected server error occurred. Try again or contact support if needed.
      </td>

      <td>
        `{"status": "fail", "error": "Something went wrong!", "code": 109}`
      </td>
    </tr>

    <tr>
      <td>
        116
      </td>

      <td>
        413
      </td>

      <td>
        `PAYLOAD_LENGTH_EXCEEDED_WITH_ATTACHMENT`
      </td>

      <td>
        The payload size<br />exceeds the permissible limit when an attachment is included. Ensure the<br />attachment does not exceed 1 MB, and the total payload does not exceed<br />1.5 MB.
      </td>

      <td>
        `{"status": "fail", "error": "The payload length exceeds the 
                                                        permissible limit", "code": 116}`
      </td>
    </tr>
  </tbody>
</Table>

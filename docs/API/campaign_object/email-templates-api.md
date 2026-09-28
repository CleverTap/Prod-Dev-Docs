---
title: Email Templates API
excerpt: >-
  Understand how to import email templates outside the CleverTap dashboard and
  manage them from the CleverTap dashboard.
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

This document provides information about the API that helps import from outside the CleverTap dashboard and manage them in the CleverTap Content Manager System.

The following APIs serve a specific purpose in the template management lifecycle:

- [Create a Template](doc:email-templates-api#create-a-template): Import a new email template with metadata, HTML content, and sender details into CleverTap CMS.
- [Get Template](doc:email-templates-api#get-template): Retrieve templates (list or single) along with their details and HTML body for review or editing.
- [Update Template by ID](doc:email-templates-api#update-template-by-id): Modify an email template, including HTML content, metadata, or labels.
- [Delete Template](doc:email-templates-api#delete-template-api): Remove one or more email templates from CleverTap CMS.

# Create a Template

The `email/templates/create`  API creates an email template on the CleverTap dashboard. When the request is made, it sends a payload with template HTML and other template properties.


<Image src="https://files.readme.io/5709059d041bbc762c3835e70c983a507b7005a029b3511d1f1b930880f00125-Email_Template_API.png" alt="Create an Email Template Using Template API" align="center" width="85%" border={true} />


Create Template API Request-Response Flow

## Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/email/templates/create](https://in1.api.clevertap.com/v1/email/templates/create)

### Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

## HTTP Method

POST

## Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

## Body Parameters

The following are the body parameters included in the JSON request payload:

<Table align={["left","left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        **Parameter**
      </th>

      <th>
        **Sub-Parameter**
      </th>

      <th>
        **Description**
      </th>

      <th>
        **Type**
      </th>

      <th>
        **Sample Value**
      </th>

      <th>
        **Required / Optional**
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `templateName`
      </td>

      <td>
        —
      </td>

      <td>
        Unique name that identifies the email template. Must be unique within the CleverTap account.
      </td>

      <td>
        String
      </td>

      <td>
        `template-name-01`
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `description`
      </td>

      <td>
        —
      </td>

      <td>
        A brief description of the template.
      </td>

      <td>
        String
      </td>

      <td>
        `description-01`
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `templateData`
      </td>

      <td>

      </td>

      <td>
        Contains the HTML body, AMP subtype, and sender details of the template.
      </td>

      <td>
        Object
      </td>

      <td>
        —
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `subType`
      </td>

      <td>
        Specifies the email subtype, for example, `amp` for AMP emails.
      </td>

      <td>
        String
      </td>

      <td>
        `amp`
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `body`
      </td>

      <td>
        The HTML content of the email template.
      </td>

      <td>
        String
      </td>

      <td>
        `<html>...</html>`
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `ampBody`
      </td>

      <td>
        The AMP HTML content of the email. **Mandatory** if `templateData.subType` is `amp`; must be left blank otherwise.
      </td>

      <td>
        String
      </td>

      <td>
        `<html>...</html>\`
      </td>

      <td>
        Conditional
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `senderDetail`
      </td>

      <td>
        Object containing sender information:<ul><li>`fromName`: Name of the sender.</li><li>`fromEmail`: Sender’s email address.</li><li>`replyToEmail`: Reply-to email address.</li><li>`subject`: Email subject line.</li><li>`plainText`: Plain-text fallback version of the email body, used by clients that cannot render HTML.</li><li>`preheader`: Preheader text of the email.</li><li>`ccEmails`: Array of CC email addresses.</li><li>`bccEmails`: Array of BCC email addresses.</li><li>`attachments`: Array of file objects to attach to the email.  Each object represents a file and contains the following:

        - `fileName`: The display name of the attachment, including the file extension (for example, invoice.pdf).
        - `fileUrl`: The publicly accessible URL from which the system fetches the file to include as an attachment in the campaign email.</li></ul>
      </td>

      <td>
        Object
      </td>

      <td>
        See sample payload
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `createdBy`
      </td>

      <td>
        —
      </td>

      <td>
        Email address of the template creator (partner user).
      </td>

      <td>
        String
      </td>

      <td>
        `email@email.com`
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `teamName`
      </td>

      <td>
        —
      </td>

      <td>
        Name of the team to which the template belongs. Team names are case-sensitive. Each template can belong to only one team.<br /><br />Note: This parameter is available only if Teams is enabled for your account.
      </td>

      <td>
        String
      </td>

      <td>
        Growth Marketing
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `partner`
      </td>

      <td>
        —
      </td>

      <td>
        Identifier of the third-party integration importing the template. Use the partner name registered with CleverTap, for example, taxi-for-email.

        **Note**: This parameter is optional and is to be used for partner integrations only. If you pass a partner name that is not registered with CleverTap, the request fails with a 400 error.&#x20;
      </td>

      <td>
        String
      </td>

      <td>
        `taxi-for-email`
      </td>

      <td>
        Optional (for partners only)
      </td>
    </tr>
  </tbody>
</Table>

<Callout icon="📘" theme="info">
  #### **Key Points to Remember**

  - The maximum supported HTML payload size is 1 MB. Email templates are typically lightweight since most content is text-based, while images or media assets are referenced via CDN URLs provided by the template editing tool.
  - CleverTap supports Liquid syntax for personalizing message content within the `templateData.body` parameter. The properties are derived from the profile schema in the CleverTap account. For more information, refer to [Personalize Message](https://docs.clevertap.com/docs/personalize-message-all).
</Callout>

## Example Request

```json Without Personalization
{
  "templateName": "Promotional Offer",
  "description": "description of template",
  "templateData": {
    "subType": "",
    "body": "<html><p> Hello World </p></html>",
    "senderDetail": {
      "fromName": "from-name",
      "fromEmail": "hello@gmail.com",
      "replyToEmail": "no-reply@email.com",
      "subject": "Subject 101",
      "preheader": "Learn about emails",
      "ccEmails": ["email1"],
      "bccEmails": ["email2"]
    }
  },
  "createdBy": "partneremail@email.com",
  "teamName": "Growth Marketing"
}
```
```json With Personalization
{
    "templateName": "July Statement Template",
    "description": "Monthly billing statement email",
    "partner": "internal",
    "teamName": "Email Pod",
    "createdBy": "hello@gmail.com",

    "templateData": {
      "subType": "html",
      "body": "<html><body>Hi {{ Profile.name | default: \"name\" }}, your statement is ready.</body></html>",
      "ampBody": "<!doctype html><html ⚡4email data-css-strict><head><meta         charset="utf-8"><style amp4email-boilerplate>body{visibility:hidden}</style></head><body>Hello world!</body></html>",
      "senderDetail": {
        "fromName": "CleverTap",
        "fromEmail": "billing@yourdomain.com",
        "replyToEmail": "support@yourdomain.com",
        "subject": "Your July statement is ready",
        "plainText": "Hi, your July statement is ready.",
        "preheader": "A quick summary of your account this month",
        "ccEmails": ["team@yourdomain.com"],
        "bccEmails": ["archive@yourdomain.com"],
        "attachments": [
          { "fileName": "statement.pdf", 
            "fileUrl": "https://cdn.yourdomain.com/statement.pdf" 
          }
        ]
      }
    }
}
```

## Example Response

```json
{
  "templateId": 176050815737031,
  "Name": "Promotional Offer",
  "result": "Template with name Promotional Offer created",
  "status": "success"
}
```

## Error Codes

If there are errors, you will receive a response in the following format:

| Error Code | Error Message                                                                                       | Description                                                                                   |
| :--------- | :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------- |
| 400        | An unexpected error occurred while processing the template. Please try again.                       | This error occurs when the payload is incorrect.                                              |
| 400        | Please check if mandatory properties are present. `templateName`, `templateData.body`, `createdBy`) | An error occurs when the specified mandatory properties are missing during template creation. |
| 401        | Invalid Credentials.                                                                                | Check the Account Credentials                                                                 |
| 429        | Too many requests.                                                                                  | API Request limit exceeded                                                                    |

# Get Template

The Get Template API enables partners to efficiently manage and retrieve email templates stored in CleverTap CMS.<br />This API serves two primary use cases:

- [Get List of Templates](doc:email-templates-api#get-list-of-templates)
- [Get a Specific Template](doc:email-templates-api#get-template-by-id)

You can either fetch a list of available templates (for browsing or filtering) or retrieve a specific template by the `templateId`.

## Get List of Templates

The `email/templates` API retrieves a paginated list of email templates available in your CleverTap account or via a third-party tool. It provides an overview of existing templates, helping users easily choose from pre-built templates for their email campaigns and eliminating the need for manual tracking.

When the request is made, it sends a payload with all the template properties.

### Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/email/templates](https://in1.api.clevertap.com/v1/email/templates)

### Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

## HTTP Method

GET

## Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

### Example Request

```curl
curl -X GET "https://in1.api.clevertap.com/v1/email/templates?pageNumber=1&pageSize=20" \
-H "X-CleverTap-Account-Id: <ACCOUNT-ID>" \
-H "X-CleverTap-Passcode: <ACCOUNT-PASSCODE>" \
-H "Content-Type: application/json"
```

### Example Response

```json
{
 "templates": [
    {
      "templateId": 123,
      "templateName": "Welcome Email",
      "type": "HTML",
      "path": "folder1/folder12/Promotion_Email1.html",
      "teamId": [101],
      "labels": ["label1", "label2"],
      "createdBy": "hello@email.com",
      "createdAt": "2024-02-01T06:29:48.965Z",
      "updatedBy": "hello@email.com",
      "updatedAt": "2024-03-01T06:29:48.965Z"
    },

    {
      "templateId": 124,
      "templateName": "Promotion Email",
      "type": "HTML",
      "path": "folder1/folder11/Promotion_Email.html",
      "teamId": [203],
      "labels": ["label2", "label3"],
      "createdBy": "hello@email.com",
      "createdAt": "2024-02-01T06:29:48.965Z",
      "updatedBy": "hello@email.com",
      "updatedAt": "2024-03-01T06:29:48.965Z"
    }
  ],

  "total": 25,
  "pageNumber": 1,
  "pageSize": 20
}
```

### Response Parameters

The following are the response parameters returned for a template:

| **Parameter**              | **Description**                                                                                                                       | **Type**       | **Sample Value**                        |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | -------------- | --------------------------------------- |
| `templates`                | An array containing template objects returned from the API.                                                                           | Array\[Object] | —                                       |
| `templates[].templateId`   | Team ID associated with the template.  Email templates always contain exactly one element; each template belongs to exactly one team. | Number         | `123`                                   |
| `templates[].templateName` | Name of the template.                                                                                                                 | String         | `Welcome Email`                         |
| `templates[].type`         | Template type. for example, HTML                                                                                                      | String         | `HTML`                                  |
| `templates[].path`         | File path or folder location of the template in CMS.                                                                                  | String         | `folder1/folder11/Promotion_Email.html` |
| `templates[].teamId`       | Team ID associated with the template. Each Template can belong to only one team                                                       | Array\[Number] | `[101]`                                 |
| `templates[].labels`       | Labels linked with the template.                                                                                                      | Array\[String] | `["label1", "label2"]`                  |
| `templates[].labelIds`     | Internal CleverTap label identifiers.                                                                                                 | Array\[Number] | `[125, 126]`                            |
| `templates[].createdBy`    | Email address of the user who created the template.                                                                                   | String         | `hello@email.com`                       |
| `templates[].createdAt`    | Timestamp of template creation (ISO 8601 format).                                                                                     | String         | `2024-02-01T06:29:48.965Z`              |
| `templates[].updatedBy`    | Email address of the last user who modified the template.                                                                             | String         | `hello@email.com`                       |
| `templates[].updatedAt`    | Timestamp of the last template update (ISO 8601 format).                                                                              | String         | `2024-03-01T06:29:48.965Z`              |
| `total`                    | Total number of templates matching the query.                                                                                         | Number         | `25`                                    |
| `pageNumber`               | Current page number in the paginated response.                                                                                        | Number         | `1`                                     |
| `pageSize`                 | Number of records returned in the current page.                                                                                       | Number         | `20`                                    |

### Error Codes

If there are errors, you will receive a response in the following format:

```json
{
  "status": "failure",
  "code": 400,
  "message": "Invalid request. One or more filter parameters are missing or malformed."
}
```

The following are the possible error codes:

| **Error Code** | **Message**           | **Description**                                            |
| -------------- | --------------------- | ---------------------------------------------------------- |
| 400            | Invalid parameters    | Missing or invalid filter parameters.                      |
| 401            | Authentication failed | Invalid or missing account credentials.                    |
| 403            | Access denied         | Partner not authorized to access this account.             |
| 404            | No templates found    | No matching templates were found for the provided filters. |
| 429            | Too many requests     | Rate limit exceeded. Retry after some time.                |
| 500            | Internal server error | Unexpected error while fetching templates.                 |

## Get Template by ID

The Get Template by ID API retrieves the complete details of a specific email template stored in CleverTap CMS.<br />By passing the templateId in the request, this API returns all metadata and content fields associated with that template, including HTML, Sender Details, and Labels.

This API is commonly used when a user needs to preview, edit, or reuse a previously created email template for new campaigns.

### Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/email/templates/\<templateId>](https://in1.api.clevertap.com/v1/email/templates/\<templateId>)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

## HTTP Method

GET

## Headers

Refer to [Headers](doc:common-api-components#headers) for more details.

## Request Parameters

The following parameters must be passed to retrieve a specific email template:

| **Parameter** | **Description**                          | **Type** | **Sample Value** | **Required / Optional** |
| ------------- | ---------------------------------------- | -------- | ---------------- | ----------------------- |
| `templateId`  | Unique identifier of the email template. | Number   | `123456789`      | **Required**            |

### Example Request

```curl
curl -X GET "https://in1.api.clevertap.com/v1/email/templates/123456789" \
-H "X-CleverTap-Account-Id: <ACCOUNT-ID>" \
-H "X-CleverTap-Passcode: <ACCOUNT-PASSCODE>" \
-H "Content-Type: application/json"
```

### Example Response

```json
{
  "templateId": 123456789,
  "templateName": "string",
  "type": "html",
  "description": "description of template",
  "path": "/folder1/folder11",
  "teamId": [101],

  "templateData": {
    "subType": "amp",
    "body": "<body></body>",
    "senderDetail": {
      "fromName": "string",
      "fromEmail": "string",
      "replyToEmail": "string",
      "subject": "string",
      "preheader": "string",
      "ccEmails": [
        "string"
      ],
      "plainText": "string",
      "bccEmails": ["string"],
      "attachments": [
        { "fileName": "string", 
          "fileUrl": "string" 
        }
      ]
    }
  },

  "labels": [
    "label1",
    "label2"
  ],

  "createdAt": "2024-02-01T06:29:48.965Z",
  "createdBy": "someemail@email.com",
  "updatedAt": "2024-02-01T06:29:48.965Z",
  "updatedBy": "someemail@email.com"
}
```

### Response Parameters

The following parameters are returned for the template:

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Sub-Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Sample Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `templateId`
      </td>

      <td>

      </td>

      <td>
        Uniquely identifies the template
      </td>

      <td>
        Number
      </td>

      <td>
        123455678
      </td>
    </tr>

    <tr>
      <td>
        `templateName`
      </td>

      <td>

      </td>

      <td>
        Indicates the template name
      </td>

      <td>
        String
      </td>

      <td>
        template-01
      </td>
    </tr>

    <tr>
      <td>
        type
      </td>

      <td>

      </td>

      <td>
        Template type.&#x20;

        - `html` for templates created via this API.
        - `Drag & Drop` for templates built in the dashboard's drag-and-drop editor.
      </td>

      <td>
        String
      </td>

      <td>
        html
      </td>
    </tr>

    <tr>
      <td>
        `description`
      </td>

      <td>

      </td>

      <td>
        Provides more information about the template you want to create
      </td>

      <td>
        String
      </td>

      <td>
        template-desc
      </td>
    </tr>

    <tr>
      <td>
        `path`
      </td>

      <td>

      </td>

      <td>
        Indicates the directory in CMS where the template is located.
      </td>

      <td>
        String
      </td>

      <td>
        f1/f2
      </td>
    </tr>

    <tr>
      <td>
        `teamId`
      </td>

      <td>

      </td>

      <td>
        Team ID that the template belongs to. Each Template can belong to only one team.
      </td>

      <td>
        Array\[Number]
      </td>

      <td>
        \[101]
      </td>
    </tr>

    <tr>
      <td>
        `templateData`
      </td>

      <td>

      </td>

      <td>

      </td>

      <td>
        Object
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `subType`
      </td>

      <td>
        Specifies the email subtype, for example, `amp` for AMP emails.
      </td>

      <td>
        String
      </td>

      <td>
        amp or blank
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `body`
      </td>

      <td>
        The HTML content of the email template.
      </td>

      <td>
        String
      </td>

      <td>
        HTML
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        `senderDetail`
      </td>

      <td>
        <ul><li> `fromName`: A string that indicates the name of the sender </li> <li> `fromEmail`: A string that indicates the email address of the sender </li><li>`replyToEmai`l: A string that indicates the email address of the recipient. </li><li>`subject` : A string that indicates the subject of the email.</li><li>`plainText`:  Plain-text fallback version of the email body.</li><li> `preheader`: A string that indicates the email preheader text.</li><li> `ccEmails`: Indicates a list (an array) of email addresses to be included in the CC field.</li><li>`bccEmails`: Indicates a list (an array) of email addresses to be included in the BCC field. </li><li>attachments: Array of file objects (fileName, fileUrl) attached to the email.</li> </ul>
      </td>

      <td>
        Object
      </td>

      <td>
        Check the payload given below
      </td>
    </tr>

    <tr>
      <td>
        `labels`
      </td>

      <td>

      </td>

      <td>
        Labels linked with the template.
      </td>

      <td>
        Array\[String]
      </td>

      <td>
        \["label1", "label2"]
      </td>
    </tr>

    <tr>
      <td>
        `createdAt`
      </td>

      <td>

      </td>

      <td>
        Indicates the date and time of template creation in ISO 8601 format.
      </td>

      <td>
        String
      </td>

      <td>
        2024-02-01T06:29:48.965Z
      </td>
    </tr>

    <tr>
      <td>
        `createdBy`
      </td>

      <td>

      </td>

      <td>
        Indicates the email address of the email template creator.
      </td>

      <td>
        String
      </td>

      <td>
        [email1@email.com](mailto:email1@email.com)
      </td>
    </tr>

    <tr>
      <td>
        `updatedBy`
      </td>

      <td>

      </td>

      <td>
        Indicates the email address of the person who last updated the email template.
      </td>

      <td>
        String
      </td>

      <td>
        [email2@email.com](mailto:email2@email.com)
      </td>
    </tr>

    <tr>
      <td>
        `updatedAt`
      </td>

      <td>

      </td>

      <td>
        Indicates the date and time when the email template was last updated in ISO 8601 format.
      </td>

      <td>
        String
      </td>

      <td>
        2024-02-01T06:29:48.965Z
      </td>
    </tr>
  </tbody>
</Table>

### Error Codes

If there are errors, you will receive a response in the following format:

```json
{
  "status": "failure",
  "code": 400,
  "message": "Invalid request. The provided templateId is missing or invalid."
}
```

The following are the possible error codes:

| Error Code | **Error Message**                               | **Description**                                                                                                |
| ---------: | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
|    **400** | Invalid request. Missing or invalid templateId. | The request payload is invalid, contains incorrect parameters, or the `templateId` does not exist.             |
|    **401** | Authentication failure.                         | The provided `X-CleverTap-Account-Id` or `X-CleverTap-Passcode` is incorrect or missing.                       |
|    **403** | Access denied. Partner not whitelisted.         | The requesting partner does not have permission to access this endpoint for the account.                       |
|    **429** | Too many requests.                              | The account has exceeded the allowed number of concurrent or rate-limited API requests (limit: 3 per account). |
|    **503** | Service unavailable. Please retry later.        | Temporary service interruption. Retry with exponential backoff.                                                |
|    **504** | Gateway timeout. Please retry later.            | The request could not be completed in time. Retry after a short delay.                                         |

# Update Template by ID

The `email/templates/update` API helps modify an existing email template by providing its unique template ID. This feature is ideal for making adjustments to a specific template. If the template ID is unknown, users can first retrieve the list of available templates using the Get Template API, identify the correct template, and then update it using the ID. This API ensures that templates remain up to date and aligned with the latest campaign requirements.

The payload must include `templateId`, `updatedBy`, `templateName`, and `templateData.body` as mandatory fields, along with any other fields that need to be updated.

### Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/email/templates/update](https://in1.api.clevertap.com/v1/email/templates/update)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer to [Headers](doc:common-api-components#headers) for more details.

## Body Parameters

Send only the fields you want to update. If updating content, the `templateData.body` parameter must be a JSON-escaped HTML string.

| **Parameter**  | Sub-Parameter               | **Description**                                                                                                                                                             | **Type**        | **Sample Value**                            | **Required / Optional**            |          |
| -------------- | :-------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------------------------------------------- | ---------------------------------- | -------- |
| `templateId`   |                             | Unique identifier of the template to update.                                                                                                                                | Number          | `123456789`                                 | **Required**                       |          |
| `templateName` |                             | Updated name of the template.                                                                                                                                               | String          | `template-01`                               | **Required**                       |          |
| `description`  |                             | Updated description of the template.                                                                                                                                        | String          | `Updated promotional email`                 | Optional                           |          |
| `path`         |                             | Updated CMS folder path for the template.                                                                                                                                   | String          | `/folder1/folder11`                         | Optional                           |          |
| `templateData` |                             | Contains the content and sender metadata. Include only the fields you want to update.                                                                                       | Object          | —                                           | Optional                           |          |
|                | `subType`                   | Specifies the email subtype, for example, `amp` for AMP emails.                                                                                                             | String          | blank or `amp`                              | Optional                           |          |
|                | `body`                      | The HTML content of the email template.                                                                                                                                     | String          | `<html><body>Updated content</body></html>` | **Required** (if updating content) |          |
|                | `senderDetail`              | Object containing sender information.                                                                                                                                       | Object          | —                                           | Optional                           |          |
|                | `senderDetail.fromName`     | Name of the sender.                                                                                                                                                         | String          | `CleverTap Marketing`                       | Optional                           |          |
|                | `senderDetail.fromEmail`    | Sender’s email address.                                                                                                                                                     | String          | `noreply@clevertap.com`                     | Optional                           |          |
|                | `senderDetail.replyToEmail` | Reply-to email address.                                                                                                                                                     | String          | `support@clevertap.com`                     | Optional                           |          |
|                | `senderDetail.subject`      | Subject of the email.                                                                                                                                                       | String          | `Your Spring Offers Are Here!`              | Optional                           |          |
|                | `senderDetail.preheader`    | Preheader text of the email.                                                                                                                                                | String          | `Exclusive savings inside`                  | Optional                           |          |
|                | `senderDetail.plainText`    | Plain-text version of the email.                                                                                                                                            | String          | Hello \{\{ Profile.name                     | default: "User" \}\}               | Optional |
|                | `senderDetail.ccEmails`     | List of CC recipients.                                                                                                                                                      | Array of String | `["team@clevertap.com"]`                    | Optional                           |          |
|                | `senderDetail.bccEmails`    | List of BCC recipients.                                                                                                                                                     | Array of String | `["analytics@clevertap.com"]`               | Optional                           |          |
| `partner`      |                             | Identifier for the partner tool updating the template.                                                                                                                      | String          | `taxi-for-email`                            | Optional                           |          |
| `teamName`     |                             | Name of the team the template belongs to. If provided, it is validated against the template's current team; a mismatch fails the request. Omit to leave the team unchanged. | String          | Growth Marketing                            | Optional                           |          |
| `updatedBy`    |                             | Email address of the user performing the update.                                                                                                                            | String          | `editor@partner.com`                        | **Required**                       |          |

## Example Request

```curl
curl -X POST "https://in1.api.clevertap.com/v1/email/templates/update" \
-H "X-CleverTap-Account-Id: <ACCOUNT-ID>" \
-H "X-CleverTap-Passcode: <ACCOUNT-PASSCODE>" \
-H "Content-Type: application/json" \
-d '{
  "templateId": 176050758737905,
  "templateName": "template-01",
  "description": "description of template",
  "templateData": {
    "subType": "",
    "body": "<html></html>",
    "senderDetail": {
      "fromName": "from-name",
      "fromEmail": "from@email.com",
      "replyToEmail": "no-reply@email.com",
      "subject": "Subject 101",
      "preheader": "Learn about emails",
      "ccEmails": ["email1"],
      "bccEmails": ["email2"]
    }
  },
  "labels": ["label1", "label2"],
  "partner": "taxi-for-email",
  "teamName": "Growth Marketing",
  "updatedBy": "editor@partner.com"
  }
```

## Example Response

```json
{
    "templateId": 176050758737905,
    "Name": "Promotional Offer",
    "result": "Template with name Promotional Offer updated",
    "status": "success"
}
```

## Error Codes

If there are errors, you will receive a response in the following format:

| **HTTP Status** | **Error Message**                                                                                                 | **Description**                                                                                                                             |
| --------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 400             | Invalid request                                                                                                   | Invalid ID or payload. Examples include missing or invalid HTML, missing or duplicate `templateName` (if provided), or missing `updatedBy`. |
| 400             | Please check if mandatory properties are present. `templateId`, `templateName`, `templateData.body`, `updatedBy`) | Missing mandatory properties for a template update.                                                                                         |
| 401             | Authentication failure                                                                                            | Invalid or missing `X-CleverTap-Account-Id` or `X-CleverTap-Passcode`.                                                                      |
| 429             | Too many requests                                                                                                 | Rate limit exceeded. A maximum of **3 concurrent requests per account** is allowed.                                                         |

# Delete Template API

The Delete Template API enables you to remove expired or obsolete email templates from CleverTap CMS. Once deleted, templates are permanently removed from the CMS section and the Saved Templates section in the Email Campaign on the CleverTap dashboard. If the template ID is unknown, users can first retrieve the list of available templates using the Get Template API, identify the correct template, and then delete it using the ID.

Campaigns or journeys that already use a deleted template remain unaffected.

## Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/email/templates/delete](https://in1.api.clevertap.com/v1/email/templates/delete)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer to [Headers](doc:common-api-components#headers) for more details.

## Body Parameters

| **Parameter** | **Description**                                                                                          | **Type**                   | **Sample Value**                     | **Required / Optional** |
| ------------- | -------------------------------------------------------------------------------------------------------- | -------------------------- | ------------------------------------ | ----------------------- |
| `_id`         | The unique identifier(s) of the template(s) is to be deleted. Can accept a single ID or an array of IDs. | Number or Array of Numbers | `[173885007188151, 173885007188152]` | Required                |

## Example Request

```curl
curl -X POST "https://in1.api.clevertap.com/v1/email/templates/delete" \
  -H "X-CleverTap-Account-Id: <ACCOUNT-ID>" \
  -H "X-CleverTap-Passcode: <ACCOUNT-PASSCODE>" \
  -H "Content-Type: application/json" \
  -d '{
    "_id": [173885007188151, 173885007188152]
  }'
```

## Example Response

```json
{
  "message": "Template(s) deleted successfully.",
  "status": "success"
}
```

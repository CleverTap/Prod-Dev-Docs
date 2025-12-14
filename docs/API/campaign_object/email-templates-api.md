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

- **[Create a Template](doc:email-templates-api#create-a-template)**: Import a new email template with metadata, HTML content, and sender details into CleverTap CMS.
- **[Get Template](doc:email-templates-api#get-template)**: Retrieve templates (list or single) along with their details and HTML body for review or editing.
- **[Update Template by ID](doc:email-templates-api#update-template-by-id)**: Modify an email template, including HTML content, metadata, or labels.
- **[Delete Template](doc:email-templates-api#delete-template-api)**: Remove one or more email templates from CleverTap CMS.

# Create a Template

The `email/templates/create` API  creates an email template on the CleverTap dashboard. When the request is made, it sends a payload with template HTML and other template properties.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/05827cb61e68401b28295a9b99eb7e8ba1df9fc972075f7efa51f6f96d21f95f-POST-removebg-preview.png",
        "",
        "Create an Email Template Using Template API"
      ],
      "align": "center",
      "sizing": "85% ",
      "border": true,
      "caption": "Create Template API Request-Response Flow"
    }
  ]
}
[/block]


## Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/v1/email/templates/create>

### Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

## HTTP Method

POST

## Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

## Body Parameters

The following are the body parameters included in the JSON request payload:

[block:parameters]
{
  "data": {
    "h-0": "**Parameter**",
    "h-1": "**Sub-Parameter**",
    "h-2": "**Description**",
    "h-3": "**Type**",
    "h-4": "**Sample Value**",
    "h-5": "**Required / Optional**",
    "0-0": "`templateName`",
    "0-1": "—",
    "0-2": "Unique name that identifies the email template. Must be unique within the CleverTap account.",
    "0-3": "String",
    "0-4": "`template-name-01`",
    "0-5": "Required",
    "1-0": "`description`",
    "1-1": "—",
    "1-2": "A brief description of the template.",
    "1-3": "String",
    "1-4": "`description-01`",
    "1-5": "Optional",
    "2-0": "`templateData`",
    "2-1": "",
    "2-2": "Contains the HTML body, AMP subtype, and sender details of the template.",
    "2-3": "Object",
    "2-4": "—",
    "2-5": "Required",
    "3-0": "",
    "3-1": "`subType`",
    "3-2": "Specifies the email subtype, for example, `amp` for AMP emails.",
    "3-3": "String",
    "3-4": "`amp`",
    "3-5": "Optional",
    "4-0": "",
    "4-1": "`body`",
    "4-2": "The HTML content of the email template.",
    "4-3": "String",
    "4-4": "`<html>...</html>`",
    "4-5": "Required",
    "5-0": "",
    "5-1": "`senderDetail`",
    "5-2": "Object containing sender information: <ul><li>`fromName`: Name of the sender.</li><li>`fromEmail`: Sender’s email address.</li><li>`replyToName`: Display name for reply-to address.</li><li>`replyToEmail`: Reply-to email address.</li><li>`subject`: Email subject line.</li><li>`preheader`: Preheader text of the email.</li><li>`ccEmails`: Array of CC email addresses.</li><li>`bccEmails`: Array of BCC email addresses.</li><li>`annotationMeta`: Gmail annotation metadata in HTML format.</li></ul>",
    "5-3": "Object",
    "5-4": "See sample payload",
    "5-5": "Optional",
    "6-0": "`partner`",
    "6-1": "—",
    "6-2": "Partner identifier (name of the third-party tool importing the template).",
    "6-3": "String",
    "6-4": "`taxi-for-email`",
    "6-5": "Optional (for partners only)",
    "7-0": "`createdBy`",
    "7-1": "—",
    "7-2": "Email address of the template creator (partner user).",
    "7-3": "String",
    "7-4": "`email@email.com`",
    "7-5": "Required",
    "8-0": "`teamName`",
    "8-1": "—",
    "8-2": "Name of the team to which the template belongs. Team names are case-sensitive. Each template can belong to only one team.",
    "8-3": "String",
    "8-4": "Growth Marketing",
    "8-5": "Required"
  },
  "cols": 6,
  "rows": 9,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


> 📘 Key Points to Remember
> 
> - The maximum supported HTML payload size is 1 MB. Email templates are typically lightweight since most content is text-based, while images or media assets are referenced via CDN URLs provided by the template editing tool.
> - CleverTap supports Liquid syntax for personalizing message content within the `templateData.body` parameter. The properties are derived from the profile schema in the CleverTap account. For more information, refer to [Personalize Message](https://docs.clevertap.com/docs/personalize-message-all).

## Example Request

```json Without Personalization
{
  "templateName": "template-01",
  "description": "description of template",
  "templateData": {
    "subType": "",
    "body": "<html></html>",
    "senderDetail": {
      "fromName": "from-name",
      "fromEmail": "from@email.com",
      "replyToName": "reply-name",
      "replyToEmail": "no-reply@email.com",
      "subject": "Subject 101",
      "preheader": "Learn about emails",
      "ccEmails": [
        "email1"
      ],
      "bccEmails": [
        "email2"
      ],
      "annotationMeta": "<div itemscope itemtype=\"http://schema.org/DiscountOffer\"><meta itemprop=\"description\" content=\"200OFF\"><meta itemprop=\"discountCode\" content=\"200OFF\"></div>"
    }
  },
  "partner": "taxi-for-email",
  "createdBy": "partneremail@email.com",
  "teamName": "Growth Marketing"
}
```
```json With Personalization
{
  "description": "description of template",
  "templateName": "Demo Template",
  "type": "HTML",
  "path": "/",
  "templateData": {
    "body": "<html lang=\"en\" xmlns:o=\"urn:schemas-microsoft-com:office:office\" xmlns:v=\"urn:schemas-microsoft-com:vml\">\n<head>\n\t<title></title>\n\t<style type=\"text/css\">*{box-sizing:border-box}body{margin:0;padding:0}a[x-apple-data-detectors]{color:inherit!important;text-decoration:inherit!important}#MessageViewBody a{color:inherit;text-decoration:none}p{line-height:inherit}.desktop_hide,.desktop_hide table{mso-hide:all;display:none;max-height:0;overflow:hidden}.image_block img+div{display:none} @media (max-width:520px){.mobile_hide{display:none}.row-content{width:100%!important}.stack .column{width:100%;display:block}.mobile_hide{min-height:0;max-height:0;max-width:0;overflow:hidden;font-size:0}.desktop_hide,.desktop_hide table{display:table!important;max-height:none!important}}\n\t</style>\n</head>\n<body style=\"background-color:#fff;margin:0;padding:0;-webkit-text-size-adjust:none;text-size-adjust:none\">\n<table border=\"0\" cellpadding=\"0\" cellspacing=\"0\" class=\"nl-container\" role=\"presentation\" style=\"mso-table-lspace:0;mso-table-rspace:0;background-color:#fff\" width=\"100%\">\n\t<tbody>\n\t\t<tr>\n\t\t\t<td>\n\t\t\t<table align=\"center\" border=\"0\" cellpadding=\"0\" cellspacing=\"0\" class=\"row row-1\" role=\"presentation\" style=\"mso-table-lspace:0;mso-table-rspace:0\" width=\"100%\">\n\t\t\t\t<tbody>\n\t\t\t\t\t<tr>\n\t\t\t\t\t\t<td>\n\t\t\t\t\t\t<table align=\"center\" border=\"0\" cellpadding=\"0\" cellspacing=\"0\" class=\"row-content stack\" role=\"presentation\" style=\"mso-table-lspace:0;mso-table-rspace:0;color:#000;width:500px;margin:0 auto\" width=\"500\">\n\t\t\t\t\t\t\t<tbody>\n\t\t\t\t\t\t\t\t<tr>\n\t\t\t\t\t\t\t\t\t<td class=\"column column-1\" style=\"mso-table-lspace:0;mso-table-rspace:0;font-weight:400;text-align:left;padding-bottom:5px;padding-top:5px;vertical-align:top;border-top:0;border-right:0;border-bottom:0;border-left:0\" width=\"100%\">\n\t\t\t\t\t\t\t\t\t<table border=\"0\" cellpadding=\"0\" cellspacing=\"0\" class=\"heading_block block-1\" role=\"presentation\" style=\"mso-table-lspace:0;mso-table-rspace:0\" width=\"100%\">\n\t\t\t\t\t\t\t\t\t\t<tbody>\n\t\t\t\t\t\t\t\t\t\t\t<tr>\n\t\t\t\t\t\t\t\t\t\t\t\t<td class=\"pad\" style=\"text-align:center;width:100%\">\n\t\t\t\t\t\t\t\t\t\t\t\t\t<h1 style=\"margin:0;color:#555;direction:ltr;font-family:Arial,'Helvetica Neue',Helvetica,sans-serif;font-size:23px;font-weight:700;letter-spacing:normal;line-height:120%;text-align:center;margin-top:0;margin-bottom:0;mso-line-height-alt:27.599999999999998px\"><span class=\"tinyMce-placeholder\">Hello, I'm {{ Profile.name | default: \"Ajay\" }} </span></h1>\n\n\t\t\t\t</html>",
    "senderDetail": {
      "fromName": "Demo Lokalise 101",
      "subject": "Working on API",
      "plainText": "",
      "preheader": "",
      "ccEmails": [
        "email1@email2.com"
      ],
      "bccEmails": [
        "email2@email3.com"
      ],
      "annotationMeta": "\n<div itemscope itemtype=\"http://schema.org/Organization\">\n<meta itemprop=\"logo\" content=\"https://brandlogovector.com/wp-content/uploads/2021/11/CleverTap-Logo-Small.png\" />\n</div>\n<div itemscope itemtype=\"http://schema.org/DiscountOffer\">\n<meta itemprop=\"description\" content=\"100OFF {{ Profile.Amount2 | default: \"100\" }}\" />\n<meta itemprop=\"discountCode\" content=\"{{ Profile.ALL_LL_MTGL1_5_1 | default: \"dhjsdcvsjvdsjh\" }}\" />\n<meta itemprop=\"availabilityStarts\" content=\"2024-02-25T20:31:35-10:00\" />\n<meta itemprop=\"availabilityEnds\" content=\"2024-02-26T20:31:35-10:00\" />\n</div>"
    }
  },
  "createdBy": "someemail@email.com",
	"teamName": "Growth Marketing"
}
```

## Example Response

```json JSON
{
"templateId": 176050815737031,
"Name": "Promotional Offer",
"result": "Template with name Promotional Offer created",
"status". "success"
}
```

## Error Codes

If there are errors, you will receive a response in the following format:

[block:parameters]
{
  "data": {
    "h-0": "Error Code",
    "h-1": "Error Message",
    "h-2": "Description",
    "0-0": "400",
    "0-1": "An unexpected error occurred while processing the template. Please try again.",
    "0-2": "This error occurs when the payload is incorrect.",
    "1-0": "400",
    "1-1": "Please check if mandatory properties are present. (templateName/templateData  \n/body/templateId(update)/createdBy/updatedBy)",
    "1-2": "An error occurs when mandatory properties mentioned in the message are missing from the payload.",
    "2-0": "401",
    "2-1": "Invalid Credentials.",
    "2-2": "Check the Account Credentials",
    "3-0": "429",
    "3-1": "Too many requests.",
    "3-2": "API Request limit exceeded"
  },
  "cols": 3,
  "rows": 4,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


# Get Template

The Get Template API enables partners to efficiently manage and retrieve email templates stored in CleverTap CMS.  
This API serves two primary use cases:

- [Get a List of Templates](doc:email-templates-api#get-a-list-of-templates)
- [Get a Specific Template](doc:email-templates-api#get-template-by-id)

You can either fetch a list of available templates (for browsing or filtering) or retrieve a specific template by the `templateId`.

## Get a List of Templates

The `email/templates` API retrieves a paginated list of email templates available within your CleverTap account or through a third-party tool. It provides an overview of the existing templates,  helping users easily choose from pre-built templates for their email campaigns, eliminating the need for manual tracking.

When the request is made, it sends a payload with all the template properties.

### Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/v1/email/templates>

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
      "teamId": [
        101
      ],
      "labels": [
        "label1",
        "label2"
      ],
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
      "teamId": [
        203
      ],
      "labels": [
        "label2",
        "label3"
      ],
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

| **Parameter**              | **Description**                                                                 | **Type**      | **Sample Value**                        |
| -------------------------- | ------------------------------------------------------------------------------- | ------------- | --------------------------------------- |
| `templates`                | An array containing template objects returned from the API.                     | Array[Object] | —                                       |
| `templates[].templateId`   | Unique identifier of the template.                                              | Number        | `123`                                   |
| `templates[].templateName` | Name of the template.                                                           | String        | `Welcome Email`                         |
| `templates[].type`         | Template type. for example, HTML                                                | String        | `HTML`                                  |
| `templates[].path`         | File path or folder location of the template in CMS.                            | String        | `folder1/folder11/Promotion_Email.html` |
| `templates[].teamId`       | Team ID associated with the template. Each Template can belong to only one team | Array[Number] | `[101]`                                 |
| `templates[].labels`       | Labels linked with the template.                                                | Array[String] | `["label1", "label2"]`                  |
| `templates[].labelIds`     | Internal CleverTap label identifiers.                                           | Array[Number] | `[125, 126]`                            |
| `templates[].createdBy`    | Email address of the user who created the template.                             | String        | `hello@email.com`                       |
| `templates[].createdAt`    | Timestamp of template creation (ISO 8601 format).                               | String        | `2024-02-01T06:29:48.965Z`              |
| `templates[].updatedBy`    | Email address of the last user who modified the template.                       | String        | `hello@email.com`                       |
| `templates[].updatedAt`    | Timestamp of the last template update (ISO 8601 format).                        | String        | `2024-03-01T06:29:48.965Z`              |
| `total`                    | Total number of templates matching the query.                                   | Number        | `25`                                    |
| `pageNumber`               | Current page number in the paginated response.                                  | Number        | `1`                                     |
| `pageSize`                 | Number of records returned in the current page.                                 | Number        | `20`                                    |

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

The Get Template by ID API retrieves the complete details of a specific email template stored in CleverTap CMS.  
By passing the templateId in the request, this API returns all metadata and content fields associated with that template, including HTML, Sender Details, and Labels.

This API is commonly used when a user needs to preview, edit, or reuse a previously created email template for new campaigns.

### Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/email/templates/<templateID>](https://in1.api.clevertap.com/v1/email/templates/<templateID>)

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
  "type": "drag-and-drop",
  "description": "description of template",
  "path": "/folder1/folder11",
  "teamId": [
    101
  ],
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
      "bccEmails": [
        "string"
      ],
      "annotationMeta": "<div itemscope itemtype=\"http://schema.org/DiscountOffer\"><meta itemprop=\"description\" content=\"200OFF\"><meta itemprop=\"discountCode\" content=\"200OFF\"></div>"
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

| Parameter    | Sub-Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Type   | Sample Value                                |
| :----------- | :------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----- | :------------------------------------------ |
| templateId   |               | Uniquely identifies the template                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Number | 123455678                                   |
| templateName |               | Indicates the template name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | String | template-01                                 |
| description  |               | Provides more information about the template you want to create                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | String | template-desc                               |
| path         |               | Indicates the directory in CMS where the template is located.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | String | f1/f2                                       |
| teamId       |               | Team ID that the template belongs to. Each Template can belong to only one team.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Number | [101]                                       |
| templateData |               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Object |                                             |
|              | subType       | Specifies the email subtype, for example, `amp` for AMP emails.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | String | amp or blank                                |
|              | body          | The HTML content of the email template.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | String | HTML                                        |
|              | senderDetail  | <ul><li> fromName: A string that indicates the name of the sender </li> <li> fromEmail: A string that indicates the email address of the sender </li><li>replyToEmail: A string that indicates the email address of the recipient. </li> <li> repolyToName: A string that indicates the name of the recipient.</li><li>subject: A string that indicates the subject of the email.</li><li> Preheader: A string that indicates the email preheader text.</li><li> ccEmails: Indicates a list (an array) of email addresses to be included in the CC field.</li><li>bccEmails: Indicates a list (an array) of email addresses to be included in the BCC field. </li><li> gmail annotation in HTML</li></ul> | Object | Check the payload given below               |
| createdAt    |               | Indicates the date and time of template creation in 8086 ISO format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | String | 2024-02-01T06:29:48.965Z                    |
| createdBy    |               | Indicates the email address of the email template creator.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | String | [email1@email.com](mailto:email1@email.com) |
| updatedBy    |               | Indicates the email address of the person who last updated the email template.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | String | [email2@email.com](mailto:email2@email.com) |
| updatedAt    |               | Indicates the date and time when the email template was last updated in 8086 ISO format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | String | 2024-02-01T06:29:48.965Z                    |

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

| Error Code | **Error Message**                                 | **Description**                                                                                                |
| ---------: | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
|    **400** | `Invalid request. Missing or invalid templateId.` | The request payload is invalid, contains incorrect parameters, or the `templateId` does not exist.             |
|    **401** | `Authentication failure.`                         | The provided `X-CleverTap-Account-Id` or `X-CleverTap-Passcode` is incorrect or missing.                       |
|    **403** | `Access denied. Partner not whitelisted.`         | The requesting partner does not have permission to access this endpoint for the account.                       |
|    **429** | `Too many requests.`                              | The account has exceeded the allowed number of concurrent or rate-limited API requests (limit: 3 per account). |
|    **503** | `Service unavailable. Please retry later.`        | Temporary service interruption. Retry with exponential backoff.                                                |
|    **504** | `Gateway timeout. Please retry later.`            | The request could not be completed in time. Retry after a short delay.                                         |

# Update Template by ID

The `email/templates/update` API helps modify an existing email template by providing its unique template ID. This feature is ideal for making adjustments to a specific template. If the template ID is unknown, users can first retrieve the list of available templates using the Get Template API, identify the correct template, and then use the ID to update it. This API ensures that templates remain up-to-date and aligned with the latest campaign requirements.

The payload must include the `templateId`  and `updatedBy` as a mandatory field along with fields that need to be updated.

### Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/v1/email/templates/update>

### Region

Refer to [Region](doc:common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer to [Headers](doc:common-api-components#headers) for more details.

## Body Parameters

Send only the fields you want to update. If updating content, the `templateData.body` parameter must be a JSON-escaped HTML string.

| **Parameter**  | Sub-Parameter                 | **Description**                                                                       | **Type**        | **Sample Value**                                                      | **Required / Optional**            |          |
| -------------- | :---------------------------- | ------------------------------------------------------------------------------------- | --------------- | --------------------------------------------------------------------- | ---------------------------------- | -------- |
| `templateId`   |                               | Unique identifier of the template to update.                                          | Number          | `123456789`                                                           | **Required**                       |          |
| `templateName` |                               | Updated name of the template.                                                         | String          | `template-01`                                                         | **Required**                       |          |
| `description`  |                               | Updated description of the template.                                                  | String          | `Updated promotional email`                                           | Optional                           |          |
| `path`         |                               | Updated CMS folder path for the template.                                             | String          | `/folder1/folder11`                                                   | Optional                           |          |
| `templateData` |                               | Contains the content and sender metadata. Include only the fields you want to update. | Object          | —                                                                     | Optional                           |          |
|                | `subType`                     | Specifies the email subtype, for example, `amp` for AMP emails.                       | String          | blank or `amp`                                                        | Optional                           |          |
|                | `body`                        | The HTML content of the email template.                                               | String          | `<html><body>Updated content</body></html>`                           | **Required** (if updating content) |          |
|                | `senderDetail`                | Object containing sender information.                                                 | Object          | —                                                                     | Optional                           |          |
|                | `senderDetail.fromName`       | Name of the sender.                                                                   | String          | `CleverTap Marketing`                                                 | Optional                           |          |
|                | `senderDetail.fromEmail`      | Sender’s email address.                                                               | String          | `noreply@clevertap.com`                                               | Optional                           |          |
|                | `senderDetail.replyToName`    | Display name for reply-to.                                                            | String          | `Support Team`                                                        | Optional                           |          |
|                | `senderDetail.replyToEmail`   | Reply-to email address.                                                               | String          | `support@clevertap.com`                                               | Optional                           |          |
|                | `senderDetail.subject`        | Subject of the email.                                                                 | String          | `Your Spring Offers Are Here!`                                        | Optional                           |          |
|                | `senderDetail.preheader`      | Preheader text of the email.                                                          | String          | `Exclusive savings inside`                                            | Optional                           |          |
|                | `senderDetail.plainText`      | Plain-text version of the email.                                                      | String          | \`Hello {{ Profile.name                                               | default: "User" }}\`               | Optional |
|                | `senderDetail.ccEmails`       | List of CC recipients.                                                                | Array of String | `["team@clevertap.com"]`                                              | Optional                           |          |
|                | `senderDetail.bccEmails`      | List of BCC recipients.                                                               | Array of String | `["analytics@clevertap.com"]`                                         | Optional                           |          |
|                | `senderDetail.annotationMeta` | Gmail annotation metadata in HTML format.                                             | String          | `<div itemscope itemtype="http://schema.org/DiscountOffer">...</div>` | Optional                           |          |
| `partner`      |                               | Identifier for the partner tool updating the template.                                | String          | `taxi-for-email`                                                      | Optional                           |          |
| `updatedBy`    |                               | Email address of the user performing the update.                                      | String          | `editor@partner.com`                                                  | **Required**                       |          |

## Example Request

```curl
curl -X POST "https://in1.api.clevertap.com/v1/email/templates/update" \
  -H "X-CleverTap-Account-Id: <ACCOUNT-ID>" \
  -H "X-CleverTap-Passcode: <ACCOUNT PASSCODE>" \
  -H "Content-Type: application/json" \
  -d '{
  "templateName": "template-01",
  "description": "description of template",
  "templateData": {
    "subType": "",
    "body": "<html></html>",
    "senderDetail": {
      "fromName": "from-name",
      "fromEmail": "from@email.com",
      "replyToName": "reply-name",
      "replyToEmail": "no-reply@email.com",
      "subject": "Subject 101",
      "preheader": "Learn about emails",
      "ccEmails": [
        "email1"
      ],
      "bccEmails": [
        "email2"
      ],
      "annotationMeta": "<div itemscope itemtype=\"http://schema.org/DiscountOffer\"><meta itemprop=\"description\" content=\"200OFF\"><meta itemprop=\"discountCode\" content=\"200OFF\"></div>"
    }
  },
  "labels": [
    "label1",
    "label2"
  ],
  "partner": "taxi-for-email",
  "createdBy": "partneremail@email.com"
  }'
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

```json
{
  "status": "failure",
  "code": 400,
  "message": "Invalid request. The provided templateId is missing or invalid."
}
```

| **HTTP Status** | **Error Message**      | **Description**                                                                                                                             |
| --------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| **400**         | Invalid request        | Invalid ID or payload. Examples include missing or invalid HTML, missing or duplicate `templateName` (if provided), or missing `updatedBy`. |
| **401**         | Authentication failure | Invalid or missing `X-CleverTap-Account-Id` or `X-CleverTap-Passcode`.                                                                      |
| **429**         | Too many requests      | Rate limit exceeded. A maximum of **3 concurrent requests per account** is allowed.                                                         |

# Delete Template API

The Delete Template API enables you to remove expired or obsolete email templates from CleverTap CMS. Once deleted, templates are permanently removed from the CMS section and the Saved Templates section in the Email Campaign on the CleverTap dashboard. If the template ID is unknown, users can first retrieve the list of available templates using the Get Template API, identify the correct template, and then use the ID to delete it.

Campaigns or journeys that already use a deleted template remain unaffected.

## Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/v1/email/templates/delete>

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
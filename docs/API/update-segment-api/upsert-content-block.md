---
title: Upsert Content Block
excerpt: Learn how to localize content blocks using the Upsert API.
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

This API allows you to localize a content block on your CleverTap dashboard.  It uses the parent ID of the block that is, `pid`, to identify the content to be localized.

# Base URL

Here is the base URL:

```
https://{region}.api.example.com/v1/contentBlock/localise/upsert
```

Replace `{region}` with the region code of your account (for example, `sk1`, `us1`, and so on). To identify the API endpoint for the region of your account, refer to [Region Codes](doc:idc#api).

# HTTP Method

`POST`

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# Body Parameters

| Parameter      | Type   | Description                                                                                                                   | Sample Value                                                                 | Required |
| -------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | -------- |
| `pid`          | Number | Unique identifier for the parent content block.                                                                               | 123456789012345                                                              | Yes      |
| `locale`       | String | Language/locale code for the content. This tells your Localize solution to translate the content into the specified language. | "spanish"                                                                    | Yes      |
| `replacements` | String | Display the content and any personalization that you may have added.                                                          | "This is your personalized content {{Profile.Location \| default= "spain"}}" | Yes      |
| `name`         | String | Name of the parent content block.                                                                                             | "Localized_Block"                                                            | No       |
| `type`         | String | Type of content. Supported: "HTML", "Text".                                                                                   | "HTML"                                                                       | No       |
| `createdBy`    | String | Email or identifier of the creator.                                                                                           | [creator@example.com](mailto:creator@example.com)                            | No       |

# Create a Localized Content Block

Provide the required details to send content to [Lokalise](https://docs.lokalise.com/en/articles/1400427-what-is-lokalise). This content is then translated to the specified language (based on the locale) and imported back into the CleverTap dashboard as a new content block. The name of this new block is appended with an underscore and locale name, for example, _CyberMonday_spanish_.

## Sample Request

```bash curl
curl --location --request POST 'https://{region}.api.example.com/v1/contentBlock/localise/upsert' \
--header 'X-Account-Id: {ACCOUNT_ID}' \
--header 'X-Passcode: {PASSCODE}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "pid": 123456789012345,
    "locale": "spanish",
    "name": "Localized_Block",
    "type": "HTML",
    "createdBy": "creator@example.com", 
    "replacements": "This is your personalized content {{Profile.Location | default= "spain"}}"
}'

```

## Sample Response

```json
{
  "message": "Content Block with id 123456789098765 edited successfully", //A new content block with the localized is created on the dashboard. 
  "status": "success"
}

```

# Errors

| Response Code | Error Message                      |
| ------------- | ---------------------------------- |
| 400           | Invalid parameters provided.       |
| 401           | Unauthorized access.               |
| 403           | Content Block feature unavailable. |
| 500           | Internal server error.             |

# API FAQs

For common questions and implementation support, refer to [API FAQs](docs:api-faqs).
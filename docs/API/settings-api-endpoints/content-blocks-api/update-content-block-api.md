---
title: Update Content Block
excerpt: Learn how to update an existing Content Block.
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

This endpoint enables you to update the content in a Content Block, such as a footer or a welcome message. You can only change/update the content in a Content Block using this API. You cannot update the Content Block type.

## Base URL

Here is an example base URL:

`https://{Region}.api.clevertap.com/v1/contentBlock/update`

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload.

| **Parameter** | **Type** | **Example Value**                 | **Description**                                                                                                                       | **Required?** |
| ------------- | -------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| **id**        | Number   | 123456789                         | The unique identifier for the content block. This is mandatory for locating the specific block.                                       | Mandatory     |
| **content**   | String   | content can be HTML or plain text | The updated content for the content block. You can only update the content in the existing Content Block type, that is, Text or HTML. | Mandatory     |
| **updatedBy** | String   | `abc@gmail.com`                   | The email address of the user making the update.                                                                                      | Mandatory     |

## Example Request

Here is an example cURL request to the _Content Blocks_ API.

```curl cU
curl --location --request POST 'https://(region).clevertap.com/v1/contentBlock/update' \
--header 'X-CleverTap-Account-Id: WRK-485-456Z' \
--header 'X-CleverTap-Passcode: d23fae0ff9324f2aa071ac87a50abcfd' \
--header 'Content-Type: application/json' \
--data-raw ' {
    "id": 172594793336599,
    "content": "HTML",
    "updatedBy": "abc@gmail.com"
  }'
```

## Example Response

The following is the sample response in case of success:

```json
{
    "message": "Content Block updated",
    "status": "success"
}
```

The following is the sample response in case of an error:

```json
{
    "status": "fail",
    "error": "We couldn't find any content blocks matching your search",
    "code": 400
}
```

## Expected errors

| Response Code | Response Error                                                                            |
| :------------ | :---------------------------------------------------------------------------------------- |
| 403           | Content Blocks are not enabled on your account. Contact Support to access Content Blocks. |
| 406           | API access disabled due to overuse. Contact customer support for more details.            |
| 500           | Request couldn’t be processed due to a system error. Try again.                           |
| 400           | We couldn't find any content blocks matching your search                                  |
| 400           | Content block ID consists of only numbers. Try again.                                     |
| 400           | Parameters entered for content block creation are invalid. Try again.                     |

# API FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

---
title: Create Content Block
excerpt: Learn how to create a new content block with API.
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

This endpoint enables you to create a new Content Block. You can create either a _Text_ or an _HTML_ Content Block. You have to specify the type, name, and content for your Content Block. For example, you can create a  footer with a content block that contains links to various social media handles. 

## Base URL

Here is an example base URL:

`https://{Region}.api.clevertap.com/v1/cms/createContentBlock`

### Region

Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload.

| **Parameter** | **Type** | **Example Value**                       | **Description**                                                                                                                                                                                                                                                                                                                                | **Required?** |
| ------------- | -------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| **type**      | String   | "Text" or "HTML"                        | Specifies the format of the content. Use "Text" for plain text and "HTML" for HTML-formatted content.                                                                                                                                                                                                                                          | Mandatory     |
| **partner**   | String   | "Taxi"                                  | Identifies the partner platform or service associated with this content, such as "Taxi". Check that you have a subscription to the service or platform and it is [supported by CleverTap](https://docs.clevertap.com/docs/technology-partners).                                                                                                | Optional      |
| **name**      | String   | "Welcome Email"                         | Provides a descriptive name for the content block, such as "Welcome Email" or "Newsletter Header". Names can include letters (A-Z, a-z), numbers (0-9), underscores (\_), periods (.), hyphens (-), parentheses (()), and spaces. However, names must not begin with a special character. Additionally, the content block name must be unique. | Mandatory     |
| **content**   | String   | "Hello, [Name]! Welcome to our service" | The main body of the content. It may include personalized text or HTML.                                                                                                                                                                                                                                                                        | Mandatory     |
| **createdBy** | String   | "[abc@gmail.com](mailto:abc@gmail.com)" | Specifies the email address of the user who created the content block.                                                                                                                                                                                                                                                                         | Mandatory     |

## Example Request

Here is an example cURL request to the _Create Content Blocks_ API.

```json

curl --location --request POST 'https://(region).clevertap.com/v1/contentBlock/create' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json'
--data-raw '{
    "name": "Welcome Message",
    "content":"<html>\n <head></head>\n <body>\n  <p>Hello Good Morning!</p>\n </body>\n</html>",
    "type":"HTML",
    "createdBy":"JohnDoe"
}'
```

## Example Response

```json
{
   "message": "Content Block with name Welcome Message created"
}
```

## Expected errors

| Response Code | Response Error                                                                          |
| :------------ | :-------------------------------------------------------------------------------------- |
| 403           | Content Blocks isn’t enabled on your account. Contact Support to access Content Blocks. |
| 400           | Invalid partner name. Check spelling or ensure the partner is registered with us.       |
| 400           | Parameters entered for content block creation are invalid. Try again.                   |
| 500           | Request couldn’t be processed due to a system error. Try again.                         |

# API FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
---
title: Get Specific Content Block
excerpt: Learn how to use the Get Specific Content Block API.
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

This endpoint enables you to get a specific Content Block by its ID. 

## Base URL

Here is an example base URL :

`https://{Region}.api.clevertap.com/v1/contentBlock/info`

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

GET

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Request Parameters

| Parameter | Description                                                                            | Example Value | Required? |
| --------- | -------------------------------------------------------------------------------------- | ------------- | --------- |
| id        | The ID of the Content Block. You can only pass a single ID. The value must be numeric. | 12345667      | Mandatory |

## Example Request

Here is an example cURL request to the _Content Blocks_ API.

```curl

curl --location --request GET 'https://(region).api.clevertap.com/v1/contentBlock/list?pageNumber=2&pageSize=5&createdAtFrom=2024-01-11T06:29:48.965Z&createdAtTo=2024-11-22T06:29:48.965Z&updatedFrom=2024-01-11T06:29:48.965Z&updatedTo=2024-11-22T06:29:48.965Z' \
--header 'X-CleverTap-Account-Id: WRK-485-456Z' \
--header 'X-CleverTap-Passcode: d23fae0ff9324f2aa071ac87a50abcfd' \
--header 'Content-Type: application/json'

```

## Example Response

```json json
{
  "id": 123456789, 
  "name": "Welcome Message",
  "type" :"HTML",
  "content": "<html>\n <head></head>\n <body>\n  <p>hello hey</p>\n </body>\n</html>",
  "createdAt": "2024-02-01T06:29:48.965Z",
  "createdBy": "someemail@email.com",
  "updatedAt": "2024-02-01T06:29:48.965Z",
  "updatedBy": "someemail@email.com"
}
```

## Expected errors

| **Response Code** | **Response Error**                                                                      |
| ----------------- | --------------------------------------------------------------------------------------- |
| 403               | Content Blocks isn’t enabled on your account. Contact Support to access Content Blocks. |
| 400               | Content block ID consists of only numbers. Try again.                                   |
| 200               | Content block ID consists of only numbers. Try again.                                   |
| 200               | Content block response.                                                                 |
| 500               | Request couldn’t be processed due to a system error. Try again.                         |

# API FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
---
title: Get Content Block List
excerpt: Learn how to use the Get Content Block List API.
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

This endpoint gives you a list of all available Content Blocks based on the query. For example,  you can define the list by number of pages required, by the person who created the blocks, and so on.

## Base URL

Here is an example base URL from the account in the India region:

`https://{Region}.api.clevertap.com/v1/contentBlock/list`

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

GET

## Headers

Refer [Headers](docs:common-api-components#api-headers) for more details.

## Request Parameters

| Parameter         | Type   | Description                                                                                                                                                                                                                                           | Example                    | Optional |
| ----------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- | -------- |
| **createdAtFrom** | String | Specifies the starting date and time for the creation of content blocks. Only content blocks created on or after this date will be included in the results. The date and time must be in ISO 8601 format, that is, `YYYY-MM-DDTHH:MM:SS.sssZ`.        | `2024-03-10T08:15:00.000Z` | Yes      |
| **createdAtTo**   | String | Specifies the ending date and time for the creation of content blocks. Only content blocks created on or before this date will be included in the results. The date and time must be in ISO 8601 format, that is, `YYYY-MM-DDTHH:MM:SS.sssZ`.         | `2024-03-18T22:30:00.000Z` | Yes      |
| **updatedFrom**   | String | Specifies the starting date and time for the last update to the content blocks. Only content blocks updated on or after this date will be included in the results. The date and time must be in ISO 8601 format, that is, `YYYY-MM-DDTHH:MM:SS.sssZ`. | `2024-04-01T12:00:00.000Z` | Yes      |
| **updatedTo**     | String | Specifies the ending date and time for the last update to the content blocks. Only content blocks updated on or before this date will be included in the results. The date and time must be in ISO 8601 format, that is, `YYYY-MM-DDTHH:MM:SS.sssZ`.  | `2024-04-05T19:45:00.000Z` | Yes      |
| **pageNumber**    | Number | Specifies which page of results to retrieve when results are paginated. The default value is `1`, which represents the first page of the results. If there are multiple pages, this parameter specifies which page to display.                        | `2`                        | Yes      |
| **pageSize**      | Number | Specifies the number of content blocks to display per page in a paginated response. The default value is `20`, and the maximum is `100`. This helps control the volume of data in each response.                                                      | `33`                       | Yes      |

## Example Request

Here is an example cURL request to the _Get Content Content Blocks List_ API.

```curl
curl --location --request GET 'https://(region).api.clevertap.com/v1/contentBlock/list' \
--header 'X-CleverTap-Account-Id: WRK-485-456Z' \
--header 'X-CleverTap-Passcode: d23fae0ff9324f2aa071ac87a50abcfd' \
--header 'Content-Type: application/json'
```

## Example Response

```json
{
  "contentBlocks": [
    {
      "id": 123,
      "name": "Welcome Email",
      "type" :"html",
      "content": "<html>\n <head></head>\n <body>\n  <p>Hello Good Morning!</p>\n </body>\n</html>",
      "createdBy": "hello@email.com",
      "createdAt": "2024-02-01T06:29:48.965Z",
      "updatedBy": "hello@email.com",
      "updatedAt": "2024-03-01T06:29:48.965Z"
    },
    {
      "id": 1234,
      "name": "Welcome Email",
      "type" :"text",
       "content": "Hello {{ Profile.name | default: \"name\" }}, Good Morning!",
      "createdBy": "hello@email.com",
      "createdAt": "2024-02-01T06:29:48.965Z",
      "updatedBy": "hello@email.com",
      "updatedAt": "2024-03-01T06:29:48.965Z"
    },
    // ... more templates
  ],
  "total": 222,
  "pageNumber": 1,
  "pageSize": 20,
  "status": "success"
}
```

## Expected errors

| **Response Code** | **Response Error**                                                                      |
| ----------------- | --------------------------------------------------------------------------------------- |
| 403               | Content Blocks isn’t enabled on your account. Contact Support to access Content Blocks. |
| 400               | The minimum page size is 1, and the maximum page size is 100.                           |
| 400               | The page number can only be a positive number.                                          |
| 400               | Enter the date in ISO format. For example, 2020-24-12T11:45:50:965Z.                    |
| 200               | There are no content blocks matching your search.                                       |
| 500               | The request couldn’t be processed due to a system error. Try again.                     |

# API FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

---
title: Settings API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  pages:
    - slug: content-blocks-api
      title: Content Blocks API
      type: basic
---
# Overview

Use this API to retrieve the list of all subscription groups created on the CleverTap dashboard.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/category-groups`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

GET

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Example Request

The following is a sample request to the Get Subscription Groups API, showing the headers needed to authenticate the request.

```curl
curl -X GET "https://<region>.api.clevertap.com/category-groups" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE"
```

## Example Response

The following is a sample response.

```json
{
    "status": "success",
    "list": [
        {
            "name": "Newsletter",
            "key": 1,
            "description": "Newsletter",
            "channels": [
                "Email"
            ],
            "updated_on": 1597812749,
            "created_on": 1597812749
        },
        {
            "name": "Product Updates",
            "key": 2,
            "description": "Product Updates",
            "channels": [
                "Email"
            ],
            "updated_on": 1597826038,
            "created_on": 1597826038
        }
    ]
}
```

The following table describes the response fields returned inside each object in the `list` array.

| Field       | Type                     | Description                                                       |
| :---------- | :----------------------- | :---------------------------------------------------------------- |
| name        | string                   | Name of the subscription group.                                   |
| key         | integer                  | Numeric identifier for the subscription group.                    |
| description | string                   | Description of the subscription group.                            |
| channels    | array of strings         | Channels this group applies to (for example, `"Email"`, `"SMS"`). |
| updated\_on | integer (Unix timestamp) | Time the group was last updated, in Unix epoch seconds.           |
| created\_on | integer (Unix timestamp) | Time the group was created, in Unix epoch seconds.                |

## Errors

To know about the errors applicable to this API, refer to [API Error Cases](doc:api-error-cases). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

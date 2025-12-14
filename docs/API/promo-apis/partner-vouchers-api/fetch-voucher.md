---
title: Fetch Voucher
excerpt: >-
  Learn how to use the Fetch Voucher API to retrieve and display assigned
  vouchers to users.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The **Fetch Voucher API** enables you to retrieve voucher codes assigned to users through CleverTap’s [Partner Voucher](https://docs.clevertap.com/docs/partner-vouchers) Campaigns. These vouchers can be displayed in the user's voucher tray within your application, allowing a seamless reward experience and promoting post-campaign engagement.

# Sample UI Behavior

Here’s how businesses can implement client-side behaviors when integrating the Fetch Voucher API into their applications. These usage patterns help ensure a seamless and intuitive user experience when displaying assigned vouchers:

- **Logged-in User**: Call the API with their `identity` to show assigned vouchers.
- **Anonymous User**: Skip the API call, or show an empty tray with a message prompting login.
- **Voucher Expiry**: Use the `listExpiry` timestamp to filter out expired vouchers on the business's side.

# Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/2/promo/vouchers/assign>

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

GET

# Query Parameters

| Parameter  | Description                                                                           | Required | Data Type | Sample Value    |
| ---------- | ------------------------------------------------------------------------------------- | -------- | --------- | --------------- |
| `identity` | Unique identifier for the user. Accepts phone (E.164 format), email, or CleverTap ID. | Yes      | String    | `+919538784114` |

# Sample Request

Here is an example request to the Fetch Voucher API, including headers needed to authenticate the request:

```curl
curl -X GET "https://in1.api.clevertap.com/2/promo/vouchers/assign?identity=krishna123&campaignId=19&voucherListId=1154&uniqueKey=19" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" 
-H "Content-Type: application/json"
```

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the _Settings_ > _Project_ page on the CleverTap dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png",
        null,
        "Find CleverTap Project Details"
      ],
      "align": "center",
      "border": true,
      "caption": "Find CleverTap Project Details"
    }
  ]
}
[/block]


# Sample Responses

The Fetch Voucher API returns different types of responses based on the validity of the input and the state of the user’s assigned vouchers. Below are sample responses for common scenarios:

### Success Response

On success, the API returns a list of vouchers assigned to the user:

```json
{
    "status": "success",
    "records": [
        {
            "listExpiry": 1750789800,
            "listTag": "summersale",
            "partner": "AMAZON",
            "campaignId": 1500,
            "externalVoucherId": 1434,
            "listName": "FLAT1000",
            "listDescription": "Get flat 1500 offer on Amazon using this voucher",
            "voucherCode": "BMO9IW7QTALK"
        },
        {
            "listExpiry": 1750789800,
            "listTag": "newyearsale",
            "partner": "FLIPKART",
            "campaignId": 1501,
            "externalVoucherId": 1434,
            "listName": "FLAT20%",
            "listDescription": "Get 20% offer on electronics items",
            "voucherCode": "3248BO386IJG"
        }
    ]
}
```

| Key                 | Description                                              | Type   | Sample Value                                                 |
| ------------------- | -------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| `voucherCode`       | The voucher code assigned to the user.                   | String | `BMO9IW7QTALK`                                               |
| `listName`          | Name of the voucher list from which the code is drawn.   | String | `FLAT1000`                                                   |
| `listTag`           | Tag associated with the voucher list.                    | String | `summersale`                                                 |
| `listExpiry`        | Expiry of the voucher list in the Epoc timestamp format. | Epoch  | `1750789800`                                                 |
| `partner`           | Partner or brand name associated with the voucher.       | String | `FLIPKART`                                                   |
| `campaignId`        | Campaign ID from which the voucher was assigned.         | Number | `1500`                                                       |
| `externalVoucherId` | Voucher list ID from which the voucher code is assigned. | Number | `1434`                                                       |
| `listDescription`   | Optional description of the voucher list.                | String | `"Offer is applicable on orders above $100"` (empty if none) |

### Empty Response

If the identity is valid but no vouchers are assigned, the API returns the following response:

```json
{
    "status": "success",
    "records": []
}
```

### Error Response

If the identity is missing or invalid:

```json
{
    "status": "fail",
    "error": "Invalid identity",
    "code": 400
}
```

# Errors

To know about the errors applicable to the Fetch Voucher API, refer to [API Error Cases](doc:api-error-cases#partner-voucher-api-errors).
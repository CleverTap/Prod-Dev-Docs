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

The Fetch Voucher API enables you to retrieve voucher codes assigned to users through CleverTap's [Partner Voucher](https://docs.clevertap.com/docs/partner-vouchers) Campaigns. These vouchers can be displayed in the user's voucher tray within your application, allowing a seamless reward experience and promoting post-campaign engagement.

## Sample UI Behavior

The following are client-side behaviors to consider when integrating the Fetch Voucher API:

- **Logged-in user**: Call the API with their `identity` to show assigned vouchers.
- **Anonymous user**: Skip the API call, or show an empty tray with a message prompting login.
- **Voucher expiry**: Use the `listExpiry` timestamp to filter out expired vouchers on the business's side.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/promo/vouchers`

For region-specific endpoints, refer to [Region](doc:common-api-components#region).

## HTTP Method

GET

## Headers

For more information about API headers, refer to [Headers](doc:common-api-components#headers).

## Query Parameters

The following table lists the query parameters.

| Parameter | Description                                                                           | Required | Type    | Example Value   |
| :-------- | :------------------------------------------------------------------------------------ | :------- | :------ | :-------------- |
| identity  | Unique identifier for the user. Accepts phone (E.164 format), email, or CleverTap ID. | Required | string  | `+919538784114` |
| page      | Page number for paginated results. Defaults to 1.                                     | Optional | integer | `1`             |

## Example Request

The following is a sample request to the Fetch Voucher API, including the headers needed to authenticate it.

```curl
curl -X GET "https://<region>.api.clevertap.com/1/promo/vouchers?identity=krishna123" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

### Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to **Settings** > **Project**.


<Image src="https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png" alt="Find CleverTap Project Details" align="center" caption="Find CleverTap Project Details" border={true} />


## Example Response

The following is a sample success response. On success, the API returns a `records` array containing all vouchers assigned to the user.

```json
{
    "status": "success",
    "records": [
        {
            "listExpiry": 1750789800,
            "listTag": "summersale",
            "partner": "AMAZON",
            "campaignId": 1500,
            "listName": "FLAT1000",
            "listDescription": "Get flat 1500 offer on Amazon using this voucher",
            "voucherCode": "BMO9IW7QTALK"
        },
        {
            "listExpiry": 1750789800,
            "listTag": "newyearsale",
            "partner": "FLIPKART",
            "campaignId": 1501,
            "listName": "FLAT20%",
            "listDescription": "Get 20% offer on electronics items",
            "voucherCode": "3248BO386IJG"
        }
    ]
}
```

The following table describes the response fields.

| Field           | Description                                                               | Type                    | Example Value                           |
| :-------------- | :------------------------------------------------------------------------ | :---------------------- | :-------------------------------------- |
| voucherCode     | The voucher code assigned to the user.                                    | string                  | `BMO9IW7QTALK`                          |
| listName        | Name of the voucher list from which the code is drawn.                    | string                  | `FLAT1000`                              |
| listTag         | Tag associated with the voucher list.                                     | string                  | `summersale`                            |
| listExpiry      | Expiry of the voucher list as a Unix timestamp in seconds.                | number (Unix timestamp) | `1750789800`                            |
| partner         | Partner or brand name associated with the voucher.                        | string                  | `FLIPKART`                              |
| campaignId      | Campaign ID from which the voucher was assigned.                          | number                  | `1500`                                  |
| listDescription | Optional description of the voucher list. Empty string if not configured. | string                  | `Offer applicable on orders above $100` |

The following is a sample empty response when the identity is valid, but no vouchers are assigned.

```json
{
    "status": "success",
    "records": []
}
```

The following is a sample error response when the identity is missing or invalid.

```json
{
    "status": "fail",
    "error": "Invalid identity",
    "code": 400
}
```

## Errors

To know about the errors applicable to the Fetch Voucher API, refer to [API Error Cases](doc:api-error-cases#partner-voucher-api-errors).

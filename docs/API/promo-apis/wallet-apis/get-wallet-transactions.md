---
title: Get Wallet Transactions
excerpt: >-
  Learn how to use Get Wallet Transactions API to retrieve detailed user wallet
  transaction logs for efficient wallet management.
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

The Get Wallet Transactions API allows you to retrieve wallet transaction details for a specific user. You can integrate this API at relevant touchpoints within the application to display transaction details on the user interface.

This API provides the following information:

- **Transaction History**: Includes a comprehensive log of all wallet transactions, that is, credits, debits, and expirations processed through various sources linked to the user's wallet.
- **Promised Points Transactions**: Displays points yet to be credited to the user's wallet based on their activation date. This allows users to monitor points scheduled for future credit. For example, if a user earns 100 points for a transaction with a 7-day activation period, these points remain in the _Pending_ bucket until the activation period ends. After 7 days, they move to _Active_ status and become redeemable.
- **Expiring Points**: Displays points nearing expiration along with their expiry dates, helping users track and redeem them in time to avoid losing benefits.
- **System Keys and Metadata**: Transactions also include additional system keys such as, `saleChannel`, `locationId`, `saleAmount`, and `orderId`. A metadata object with key–value pairs is also returned, reflecting any data provided when the Debit/Credit transaction was created. If no metadata was associated, the field is returned as null.

# Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/1/promo/transactions>

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

GET

# Query Parameters

The API requires the following query parameters:

| Parameter  | Description                                                                                                                                                                                                                                                        | Type   | Required/Optional | Sample Value |
| :--------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----- | :---------------- | :----------- |
| `identity` | Identifies the user associated with the wallet.                                                                                                                                                                                                                    | String | Required          | `KMN@123`    |
| `walletId` | Unique identifier assigned to each user wallet within the application.                                                                                                                                                                                             | String | Required          | `wallet_123` |
| `page`     | Page number for paginated results. The default value is 1. The wallet transaction details are returned in a paginated format. You must pass the `page` number as a query parameter to retrieve specific pages. Each page can include a maximum of 20 transactions. | Number | Optional          | `1`          |

# Sample Request

Here is an example request to the Get Wallet Transactions API, including headers needed to authenticate the request:

```curl
curl -X GET "https://in1.api.clevertap.com/1/promo/transactions?identity=krishna123&walletId=2163" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
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


# Sample Response

The Get Wallet Transactions API response depends on query parameters, pagination settings, and transaction sources. The API returns an array of objects, with each object containing details about a specific transaction for the given `walletId`.

The following sample response returns the paginated list of transaction details:

```json
{
   "status": "success",
   "record": {
       "promisedPoints": {
           "totalPromisedPoints": 0,
           "promisedPointsList": []
       },
       "pointsExpiring": {
           "earliestExpiryTimestamp": 1754471850,
           "pointsExpiringSoon": 77,
           "pointsExpiringList": [
               {
                   "expiryTimestamp": 1754471850,
                   "points": 77
               },
               {
                   "expiryTimestamp": 1754472109,
                   "points": 7.7
               }
           ]
       },
       "allTransactions": [
           {
               "txnTimestamp": 1754386499,
               "saleAmount": 0,
               "metadata": null,
               "orderId": "",
               "saleChannel": "",
               "campaignId": 1754386284,
               "locationId": "",
               "txnSource": "SYSTEM",
               "description": "Points expired automatically by system",
               "type": "EXPIRED",
               "tnxId": "wl_wB3FNMnD0ea0QD0xwfg1W",
               "points": 259.9
           },
           {
               "txnTimestamp": 1754384850,
               "saleAmount": 1000,
               "metadata": {
                   "Key1": "Test1",
                   "key2": "Test2"
               },
               "orderId": "order123",
               "saleChannel": "UPI",
               "campaignId": 0,
               "locationId": "BAN123",
               "txnSource": "API",
               "description": "Reward points for purchase",
               "type": "CREDIT",
               "tnxId": "wl_71ezcyzGWLc0S19qeCUo1",
               "points": 77
           }
       ]
   }
}
```

The sections below provide a detailed breakdown of the response payload for the Get Wallet Transactions API.

## `pointsExpiring` Object

The **`pointsExpiring` object** returns details about reward points nearing expiry. It includes information such as the total points expiring soon, the earliest expiry date, and a detailed list of points with their respective expiry timestamps.

| Parameter                 | Description                                                                     | Type             | Sample Value             |
| ------------------------- | ------------------------------------------------------------------------------- | ---------------- | ------------------------ |
| `pointsExpiringSoon`      | Total points expiring on the earliest upcoming expiry date.                     | Numeric          | `60`                     |
| `earliestExpiryTimestamp` | The earliest expiry date of points getting expired (sorted in ascending order). | DateTime         | `"2024-12-13T12:12:12Z"` |
| `pointsExpiringList`      | List of points expiring.                                                        | Array of objects |                          |
| `points`                  | Points expiring on the respective expiry date.                                  | Numeric          | `20`                     |
| `expiryTimestamp`         | The expiry date of respective points.                                           | DateTime         | `2024-12-13T12:12:12Z`   |

## `allTransactions` Object

The **`allTransactions` object** returns the complete transaction history of the wallet, including credits, debits, expired, and pending transactions. Each transaction entry provides comprehensive details such as the transaction source, type, description, timestamp, and points involved.

[block:parameters]
{
  "data": {
    "h-0": "Response Field",
    "h-1": "Definition",
    "h-2": "Type",
    "h-3": "Sample Value",
    "0-0": "`txnSource`",
    "0-1": "Source through which the wallet transaction occurred. The following are the possible sources:<ul><li>**Campaign**: Transaction originated from a promotional campaign.</li><li>**API**: Transaction triggered via API integration.</li><li>**Manual**: Transaction added or updated manually by an administrator.</li><li>**Cashback Coupon**: Transaction that reflects cashback awarded to the user as part of a coupon redemption.</li><li>**System**: Transaction automatically generated by the system, for example, coupon expiration </li></ul>",
    "0-2": "String",
    "0-3": "`CAMPAIGN`, `API`, `MANUAL`,`CASHBACKCOUPON`, `SYSTEM`",
    "1-0": "`description`",
    "1-1": "Description of the wallet transaction.",
    "1-2": "String",
    "1-3": "`Points debited for late payment`",
    "2-0": "`type`",
    "2-1": "Wallet transaction type indicating the nature of the transaction. The following are the possible values:<ul><li>**Credit**: Points added to the wallet.</li><li>**Debit**: Points deducted from the wallet.</li><li>**Expired**: Points that have reached their expiry date and are no longer valid.</li><li>**Pending**: Points awaiting activation or clearance.</li></ul>",
    "2-2": "String",
    "2-3": "`CREDIT`, `DEBIT`, `EXPIRED`, `PENDING`",
    "3-0": "`tnxId`",
    "3-1": "Transaction ID assigned to a respective wallet transaction.",
    "3-2": "String",
    "3-3": "`wl_MRCJSqtH9akcvzvK`",
    "4-0": "`allTransactions`",
    "4-1": "List of all transactions.",
    "4-2": "Array of objects",
    "4-3": "",
    "5-0": "`points`",
    "5-1": "Wallet points debited or credited against each transaction.",
    "5-2": "Numeric",
    "5-3": "`20`",
    "6-0": "`txnTimestamp`",
    "6-1": "Date and Time when the transaction was created.",
    "6-2": "DateTime",
    "6-3": "`2024-12-13T12:12:12Z`",
    "7-0": "`orderId`",
    "7-1": "References the bill number to associate the transaction with a specific order ID for potential future refunds. This enables tracking transactions for return sales.",
    "7-2": "String",
    "7-3": "`ORD12345`",
    "8-0": "`saleChannel`",
    "8-1": "The source channel for the transaction (e.g., Mobile, Web, POS). Applies to both credit and debit transactions created via API.",
    "8-2": "String",
    "8-3": "`POS`",
    "9-0": "`locationId`",
    "9-1": "Identifier for the transaction location (e.g., store code). Applies to both credit and debit transactions created via API.",
    "9-2": "String",
    "9-3": "`BAN123`",
    "10-0": "`saleAmount`",
    "10-1": "Gross order value tied to the transaction. Applies to both credit and debit transactions.",
    "10-2": "Numeric",
    "10-3": "`2499`",
    "11-0": "`metadata`",
    "11-1": "The key-value pairs to persist with the transactions. Applies to both credit and debit transactions.",
    "11-2": "Object",
    "11-3": "`{ \"cashierId\": \"C42\", \"campaign\": \"FESTIVE25\" }`"
  },
  "cols": 4,
  "rows": 12,
  "align": [
    null,
    null,
    null,
    null
  ]
}
[/block]


> 📘 Note
> 
> - For transactions created through the Debit/Credit API, the system keys, i.e., `saleChannel`, `locationId`, `saleAmount` and any `metadata` provided will be returned in the response.
> - For expired or system-generated transactions, these fields may not apply and can appear as empty strings, i.e.,`""`, `0`, or `null`. For example, refer to the [Sample Response](doc:get-wallet-transactions#sample-response).

## `promisedPoints` Object

The **`promisedPoints` object** returns reward points that have yet to be credited to the wallet. It specifies the activation timestamp for promised points, a breakdown of individual entries, and the total promised points awaiting activation.

| Key                   | Description                                                                       | Type     | Sample Value           |
| --------------------- | --------------------------------------------------------------------------------- | -------- | ---------------------- |
| `promisedPointsList`  | List of points yet to be credited to the user wallet.                             | Array    |                        |
| `activationTimestamp` | Date and Time on which associated points will be credited or activated.           | DateTime | `2024-12-13T12:12:12Z` |
| `points`              | Points that are pending and will be activated based on the `activationTimestamp`. | Numeric  | `30`                   |
| `totalPromisedPoints` | Total points that are yet to be credited to the user wallet.                      | Numeric  | `100`                  |

# Errors

To know the errors applicable to the Get Wallet Transactions API, refer to [API Error Cases](doc:api-error-cases#wallet-api-errors).
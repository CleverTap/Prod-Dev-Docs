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

The Get Wallet Transactions API helps you to retrieve wallet transaction details for a specific user. You can integrate this API at relevant touchpoints within the application to display transaction details on the user interface.

This API provides the following information:

* **Transaction History**: Includes a comprehensive log of all wallet transactions, that is, credits, debits, and expirations processed through various sources linked to the user's wallet.
* **Promised Points**: Displays points yet to be credited to the user wallet based on their activation date. This allows users to monitor points scheduled for future credit. For example, if a user earns 100 points for a transaction with a 7-day activation period, these points remain in the _Pending_ bucket until the activation period ends. After 7 days, they move to _Active_ status and become redeemable.
* **Expiring Points**: Displays points nearing expiration along with their expiry dates, helping users track and redeem them before they expire to avoid losing benefits.
* **System Keys and Metadata**: Includes additional system keys such as, `saleChannel`, `locationId`, `saleAmount`, and `orderId`. A metadata object with key–value pairs is also returned, reflecting any data provided when the debit/credit transaction was created. If no metadata was associated, the field is returned as null.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/transactions](https://in1.api.clevertap.com/1/promo/transactions)

## Region

To identify the API endpoint for your account region, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

GET

# Query Parameters

The API requires the following query parameters:

| Parameter     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Type    | Required/Optional | Sample Value    |
| :------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :---------------- | :-------------- |
| `identity`    | Identifies the user associated with the wallet.                                                                                                                                                                                                                                                                                                                                                                                                                                      | String  | Required          | `KMN@123`       |
| `walletId`    | Unique identifier assigned to each user wallet within the application.                                                                                                                                                                                                                                                                                                                                                                                                               | String  | Required          | `wallet_123`    |
| `page`        | Page number for paginated results. The default value is 1. The wallet transaction details are returned in a paginated format. You must pass the `page` number as a query parameter to retrieve specific pages. Each page can include up to 25 transactions.                                                                                                                                                                                                                          | Number  | Optional          | `1`             |
| `type`        | The type of transaction to retrieve. Accepted values are `credit`, `debit`, `expired`, `reverse`, and `refund`. If not specified, the API returns transactions from all sources without filtering. Values are case-insensitive, meaning `credit` and `CREDIT` are treated as the same. You can pass multiple comma-separated values to filter for more than one type at once. For example, when using the filter `type=credit,debit`, it returns both credit and debit transactions. | String  | Optional          | `credit, debit` |
| `txnSource`   | The source through which the wallet transaction occurred. Accepted values are `api`, `system`, `campaign`, `manual`, `cashbackcoupon`. If not specified, the API returns transactions from all sources without filtering. Values are case-insensitive, meaning `cashbackcoupon` and `CASHBACKCOUPON` are treated as the same. Only one value is supported per request.                                                                                                               | String  | Optional          | `campaign`      |
| `saleChannel` | The sale channel recorded against credit and debit transactions. This is an open string value. Pass the value exactly as it was recorded. The filter is case-sensitive. For example, `POS` and `pos` are treated as different values. Spaces and special characters are accepted. For example, `star-pos` or `star pos`. Only one value is supported per request.                                                                                                                    | String  | Optional          | `star-pos`      |
| `campaignId`  | The numeric ID of a CleverTap promo campaign. Pass this parameter to retrieve transactions associated with a specific campaign. Only one value is supported per request.                                                                                                                                                                                                                                                                                                             | Integer | Optional          | `456`           |
| `orderId`     | The order ID recorded against credit and debit transactions. Pass this parameter to retrieve transactions associated with a specific order. Only one value is supported per request.                                                                                                                                                                                                                                                                                                 | String  | Optional          | `ORD-98123`     |
| `locationId`  | The location ID recorded against credit and debit transactions. Pass this parameter to retrieve transactions associated with a specific location. Only one value is supported per request.                                                                                                                                                                                                                                                                                           | String  | Optional          | `BAN-MG-ROAD`   |
| `from`        | Filters transactions with a timestamp on or after this value. Pass the timestamp in UTC epoch format (seconds). If `to` is not provided, the API defaults the end time to the current timestamp. Filtering is inclusive, meaning the transactions matching exactly this timestamp are included.                                                                                                                                                                                      | Integer | Optional          | `1704067200`    |
| `to`          | Filters transactions with a timestamp on or before this value. Pass the timestamp in UTC epoch format (seconds). Must be used together with `from`. Filtering is inclusive, meaning the transactions matching exactly this timestamp are included.                                                                                                                                                                                                                                   | Integer | Optional          | `1706745600`    |

<Callout icon="📘" theme="info">
  #### Note

  * All filter parameters are optional. Filters not passed, or passed with blank or null values, are silently ignored.
  * Filters only apply to the `allTransactions` object in the response. The `promisedPoints` and `pointsExpiring` objects are not affected and always return their full data.
  * When no transactions match the applied filters, the `allTransactions` array returns empty. This is not an error.
  * All filter keys work together with `AND` logic by default.
</Callout>

## Filter Behavior

The following rules apply when using query parameters to filter wallet transactions. Understanding these behaviors helps you combine filters correctly and avoid unexpected results.

| Scenario                                         | Behavior                                                                                                                                                                                                               |
| :----------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Multiple filter keys use AND logic               | When you pass multiple filter keys, the API returns only transactions that match all conditions. For example, `type=credit&txnSource=campaign` returns only credit transactions that originated from a promo campaign. |
| `type` supports multiple comma-separated values  | You can pass more than one value for type in a single request. For example, `type=credit,debit` returns both credit and debit transactions.                                                                            |
| All other filter keys support only one value     | Passing multiple values for `orderId`, `saleChannel`, `txnSource`, `campaignId`, or `locationId` returns an error.                                                                                                     |
| `type` and `txnSource` are case-insensitive      | `credit` and `CREDIT` are treated as the same. `campaign` and `CAMPAIGN` are treated as the same.                                                                                                                      |
| `saleChannel` is case-sensitive                  | `POS` and `pos` are treated as different values. Pass the value exactly as it was recorded at the time of the transaction.                                                                                             |
| Blank or null filter values are silently ignored | If a filter key is passed with no value, it does not affect the results.                                                                                                                                               |
| Filters apply only to `allTransactions`          | The `promisedPoints` and `pointsExpiring` objects in the response are not affected by any filter. They always return their full data, regardless of the filters applied.                                               |

## Date Range Behavior

The `from` and `to` parameters let you filter transactions to a specific time window. Both values must be passed in UTC epoch format (seconds). The following rules define how the API handles different date range inputs.

| Scenario                                              | Behavior                                                                                                                |
| :---------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------- |
| Both `from` and `to` provided                         | Returns transactions where `from` ≤ `txnTimestamp` ≤ `to`. Both boundaries are inclusive.                               |
| Only `from` provided, `to` omitted                    | The API automatically uses the current timestamp as `to`. Returns all transactions from the given start time up to now. |
| Only `to` provided, `from` omitted                    | Returns the `to requires from` error. `to` cannot be used without `from`.                                               |
| `from`is later than `to`                              | Returns the `Invalid date range` error                                                                                  |
| Date passed as a formatted string instead of an epoch | Returns the `Invalid date format, expected in epoch` error                                                              |
| Invalid or non-numeric epoch value                    | Returns the `Invalid date format` error                                                                                 |

For full-day filtering, pass `from` as `00:00:00` UTC and `to` as `23:59:59` UTC for the target date, converted to epoch. For example, to filter all transactions on January 1, 2024 (UTC), pass `from=1704067200&to=1704153599`.

# Sample Request

Here is an example request to the Get Wallet Transactions API, including headers needed to authenticate the request:

```curl cURL without Filter
curl -X GET "https://in1.api.clevertap.com/1/promo/transactions?identity=krishna123&walletId=2163" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" 
```
```curl cURL with Filters
curl -X GET "https://in1.api.clevertap.com/1/promo/transactions?identity=krishna123&walletId=2163&type=credit,debit&txnSource=campaign&campaignId=456&from=1704067200&to=1706745600" \
  -H "X-CleverTap-Account-Id: ACCOUNT_ID" \
  -H "X-CleverTap-Passcode: PASSCODE" \
  -H "Content-Type: application/json"

```

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the _Settings_ > _Project_ page on the CleverTap dashboard.

<Image align="center" alt="Find CleverTap Project Details" border={true} caption="Find CleverTap Project Details" src="https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png" />

# Sample Response

The Get Wallet Transactions API response depends on query parameters, pagination settings, and transaction sources. The API returns an array of objects, with each object containing details about a specific transaction for the given `walletId`.

The following sample response returns the paginated list of transaction details:

```json
{
  "status": "success",
  "record": {
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
        "type: "EXPIRED",
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
    ],
    "pagination": {
      "currentPage": 1,
      "pageSize": 25,
      "totalPages": 10,
      "totalRecords": 248,
      "hasNext": true
    },
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
    }
  }
}
```

<Callout icon="📘" theme="info">
  #### Note

  * The parameters `type`, `txnSource`, `saleChannel`, `campaignId`, `orderId`, and `locationId` in each `allTransactions` entry correspond directly to the optional query parameters you can use to filter results.
  * The `from` and `to` query parameters filter based on the `txnTimestamp` field.
  * The `promisedPoints` and `pointsExpiring` objects are not affected by any filters applied.
</Callout>

The sections below provide a detailed breakdown of the response payload for the Get Wallet Transactions API.

## `allTransactions` Object

The **`allTransactions`object** returns the complete transaction history of the wallet, including credits, debits, expired, and pending transactions. Each transaction entry provides comprehensive details such as the transaction source, type, description, timestamp, and points involved.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Sample Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `txnTimestamp`
      </td>

      <td>
        Unix timestamp (UTC epoch, in seconds) when the transaction was created.
      </td>

      <td>
        Integer
      </td>

      <td>
        `1754386499`
      </td>
    </tr>

    <tr>
      <td>
        `saleAmount`
      </td>

      <td>
        Total order amount associated with the transaction. For example, if a customer makes a purchase worth 2,499, this field reflects that value. Applies to both credit and debit transactions.
      </td>

      <td>
        Numeric
      </td>

      <td>
        `2499`
      </td>
    </tr>

    <tr>
      <td>
        `metadata`
      </td>

      <td>
        The key-value pairs to persist with the transactions. Applies to both credit and debit transactions.
      </td>

      <td>
        Object
      </td>

      <td>
        `{ "cashierId": "C42", "campaign": "FESTIVE25" }`
      </td>
    </tr>

    <tr>
      <td>
        `orderId`
      </td>

      <td>
        References the bill number to associate the transaction with a specific order ID for potential future refunds. This enables tracking transactions for return sales.
      </td>

      <td>
        String
      </td>

      <td>
        `ORD12345`
      </td>
    </tr>

    <tr>
      <td>
        `saleChannel`
      </td>

      <td>
        The source channel for the transaction (for example, Mobile, Web, POS). Applies to both credit and debit transactions created via API.
      </td>

      <td>
        String
      </td>

      <td>
        `POS`
      </td>
    </tr>

    <tr>
      <td>
        `locationId`
      </td>

      <td>
        Identifier for the transaction location (for example, store code). Applies to both credit and debit transactions created via API.
      </td>

      <td>
        String
      </td>

      <td>
        `BAN123`
      </td>
    </tr>

    <tr>
      <td>
        `txnSource`
      </td>

      <td>
        Source through which the wallet transaction occurred. The following are the possible sources:

        * **Campaign**: Transaction originated from a promotional campaign.
        * **API**: Transaction triggered via API integration.
        * **Manual**: Transaction added or updated manually by an administrator.
        * **Cashback Coupon**: A transaction that reflects cashback awarded to the user as part of a coupon redemption.
        * **System**: Transaction automatically generated by the system, for example, coupon expiration.
      </td>

      <td>
        String
      </td>

      <td>
        `CAMPAIGN`, `API`, `MANUAL`,`CASHBACKCOUPON`, `SYSTEM`
      </td>
    </tr>

    <tr>
      <td>
        `campaignId`
      </td>

      <td>
        The numeric ID of the CleverTap promo campaign associated with the transaction. For transactions not linked to a campaign, this field returns `0`.
      </td>

      <td>
        Integer
      </td>

      <td>
        `1754386284`
      </td>
    </tr>

    <tr>
      <td>
        `description`
      </td>

      <td>
        Description of the wallet transaction.
      </td>

      <td>
        String
      </td>

      <td>
        `Points debited for late payment`
      </td>
    </tr>

    <tr>
      <td>
        `type`
      </td>

      <td>
        Wallet transaction type indicating the transaction's nature. The following are the possible values:

        * **Credit**: Points added to the wallet.
        * **Debit**: Points deducted from the wallet.
        * **Expired**: Points that have reached their expiry date and are no longer valid.
        * **Pending**: Points awaiting activation or clearance.
      </td>

      <td>
        String
      </td>

      <td>
        `CREDIT`, `DEBIT`, `EXPIRED`, `PENDING`
      </td>
    </tr>

    <tr>
      <td>
        `tnxId`
      </td>

      <td>
        Transaction ID assigned to a respective wallet transaction.
      </td>

      <td>
        String
      </td>

      <td>
        `wl_MRCJSqtH9akcvzvK`
      </td>
    </tr>

    <tr>
      <td>
        `points`
      </td>

      <td>
        Wallet points debited or credited against each transaction.
      </td>

      <td>
        Numeric
      </td>

      <td>
        `20`
      </td>
    </tr>
  </tbody>
</Table>

<Callout icon="📘" theme="info">
  #### Note

  For transactions created through the Debit/Credit API, the system keys, i.e., `saleChannel`, `locationId`, `saleAmount` and any `metadata` provided will be returned in the response. For expired or system-generated transactions, these keys may not apply and can appear as empty strings, i.e.,`""`, `0`, or `null`. For example, refer to the [Sample Response](doc:get-wallet-transactions#sample-response).
</Callout>

## `pagination` Object

The `pagination` object is returned alongside the `allTransactions` array and provides details about the total volume of matching records and your current position within the paginated results. Use this object to build pagination controls in your platform's user interface. For example, displaying `Page 2 of 10` or enabling a `Next Page` button.

| Parameter      | Description                                                                                                                                                  | Type    | Sample Value |
| :------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :----------- |
| `currentPage`  | The page number of the current response. Corresponds to the `page` query parameter passed in the request.                                                    | Integer | `1`          |
| `pageSize`     | The number of records returned in the current page. The maximum is 25 records per page.                                                                      | Integer | `25`         |
| `totalPages`   | The total number of pages available for the current filter combination.                                                                                      | Integer | `10`         |
| `totalRecords` | The total number of transactions matching the applied filters across all pages.                                                                              | Integer | `248`        |
| `hasNext`      | Indicates whether there are more records available beyond the current page. `true` means additional pages exist. `false` means the current page is the last. | Boolean | `true`       |

<Callout icon="📘" theme="info">
  #### Note

  Pagination applies only to the `allTransactions` object. The `promisedPoints` and `pointsExpiring` objects always return up to 50 records each, unaffected by filters.
</Callout>

## `promisedPoints` Object

The **`promisedPoints`object** returns reward points that are yet to be credited to the wallet. It specifies the activation timestamp for promised points, a breakdown of individual entries, and the total promised points awaiting activation.

| Parameter             | Description                                                                                          | Type    | Sample Value |
| --------------------- | ---------------------------------------------------------------------------------------------------- | ------- | ------------ |
| `promisedPointsList`  | List of points yet to be credited to the user wallet.                                                | Array   |              |
| `activationTimestamp` | Unix timestamp (UTC epoch, in seconds) on which the associated points will be credited or activated. | Integer | `1754471850` |
| `points`              | Points that are pending and will be activated based on the `activationTimestamp`.                    | Numeric | `30`         |
| `totalPromisedPoints` | Total points yet to be credited to the user wallet.                                                  | Numeric | `100`        |

## `pointsExpiring` Object

The **`pointsExpiring`object** returns details about reward points nearing expiry. It includes information such as the total points expiring soon, the nearest expiry date, and a detailed list of points with their respective expiry timestamps.

| Parameter                 | Description                                                                                                                                                              | Type             | Sample Value |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------- | ------------ |
| `pointsExpiringSoon`      | Total points expiring on the nearest upcoming expiry date.                                                                                                               | Numeric          | `60`         |
| `earliestExpiryTimestamp` | Unix timestamp (UTC epoch, in seconds) of the nearest upcoming expiry date across all entries in `pointsExpiringList`.                                                   | Integer          | `1754471850` |
| `pointsExpiringList`      | List of upcoming point expiry entries, each showing the expiry timestamp and the number of points expiring on that date. Sorted in ascending order by `expiryTimestamp`. | Array of objects |              |
| `points`                  | Points expiring on the respective expiry date.                                                                                                                           | Numeric          | `20`         |
| `expiryTimestamp`         | Unix timestamp (UTC epoch, in seconds) on which the respective points expire.                                                                                            | Integer          | `1754471850` |

# Errors

To know the errors applicable to the Get Wallet Transactions API, refer to [API Error Cases](doc:api-error-cases#wallet-api-errors).

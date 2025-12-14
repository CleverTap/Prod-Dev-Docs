---
title: Get Wallet Details
excerpt: Learn how to use the Get Wallet Details API to access user wallet information.
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

The Get Wallet Details API fetches user wallet information, including the wallet balance, reward points, and pending/expired points. This API can be integrated into your application at key touch points to provide seamless access to wallet data within your application user interface.

This API is designed to offer the following two key components:

* **Wallet Definition**: Provides the following essential wallet configuration details necessary for accurate wallet identification and display within your application:
  * Wallet Name
  * Loyalty Points Name
  * Wallet ID
* **Wallet Ledger Balance**: Provides a detailed breakdown of the user's wallet activity for display within the application user interface, as follows:
  * Active Points
  * Promised Points
  * Credited Points
  * Expired Points
  * Redeemed Points

Using these insights, you can present an up-to-date and transparent view of the user's wallet activity within your application.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/wallets](https://in1.api.clevertap.com/1/promo/wallets)

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

GET

# Query Parameters

| Parameter  | Description                                     | Type   | Sample Value | Required/Optional |
| :--------- | :---------------------------------------------- | :----- | :----------- | :---------------- |
| `identity` | Identifies the user associated with the wallet. | String | `KMN@123`    | Required          |

# Sample Request

Here is an example request to the Get Wallet Details API showing the headers needed to authenticate the request:

```curl
curl -X GET "https://in1.api.clevertap.com/1/promo/wallets?identity=krishna123" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the *Settings* > *Project* page on the CleverTap dashboard.

<Image alt="Find CleverTap Project Details" align="center" border={true} src="https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png">
  Find CleverTap Project Details
</Image>

# Sample Response

The sample request returns an array of wallet objects, as shown in the following sample JSON response:

```json
{
    "status": "success",
    "records": [
        {
            "walletId": 1000,
            "totalRedeemedPoints": 0,
            "totalCreditedPoints": 0,
            "activePoints": 0,
            "walletName": "CleverTap Wallet",
            "totalPromisedPoints": 0,
            "loyaltyPointsName": "Points",
            "expiredPoints": 0
        },
        {
            "walletId": 1003,
            "totalRedeemedPoints": 0,
            "totalCreditedPoints": 0,
            "activePoints": 0,
            "walletName": "Promo Wallet",
            "totalPromisedPoints": 0,
            "loyaltyPointsName": "Promo Coins",
            "expiredPoints": 0
        },
        {
            "walletId": 1020,
            "totalRedeemedPoints": 129,
            "totalCreditedPoints": 388.579,
            "activePoints": 0,
            "walletName": "test",
            "totalPromisedPoints": 129.526,
            "loyaltyPointsName": "Offer",
            "expiredPoints": 259.053
        }
    ]
}
```

On a successful API call, the system fetches the API response and provides the following response fields passed in the request body:

| Key                   | Definition                                                                                                                 | Type    | Sample Value   |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------- | -------------- |
| `walletId`            | Unique wallet ID assigned by CleverTap for a specific wallet created on your CleverTap dashboard.                          | Number  | `1000`         |
| `walletName`          | Name of the wallet.                                                                                                        | String  | `Super wallet` |
| `loyaltyPointsName`   | Name of the currency associated with the wallet.                                                                           | String  | `Coin`         |
| `totalRedeemedPoints` | Total points redeemed from the user wallet via the [Debit Wallet API](doc:debitcredit-wallet) and the CleverTap dashboard. | Numeric | `20`           |
| `totalCreditedPoints` | Total points rewarded to the user from all sources.                                                                        | Numeric | `20`           |
| `activePoints`        | Current active wallet points held by a given user.                                                                         | Numeric | `40`           |
| `totalPromisedPoints` | Total points rewarded to a user but not yet credited to the user’s wallet.                                                 | Numeric | `10`           |
| `expiredPoints`       | Total number of unused points expired in the user's wallet.                                                                | Numeric | `5`            |

# Errors

To know the errors applicable to the Get Wallet Details API, refer to [API Error Cases](doc:api-error-cases#wallet-api-errors).

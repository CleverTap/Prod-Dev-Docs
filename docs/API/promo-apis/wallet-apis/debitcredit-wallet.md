---
title: Debit/Credit Wallet
excerpt: >-
  Learn how to use the Debit/Credit Wallet API to adjust user wallet points,
  manage loyalty rewards, and effectively offer users real-time incentives.
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

The Debit/Credit Wallet API allows you to update the points in the user's wallet by either debiting (subtracting) or crediting (adding) them. The wallet ledger balance is updated immediately after the transaction is processed.

This API provides flexibility for a range of scenarios where point adjustments are required for the user's wallet, as follows:

* **Redeeming Wallet Points in Transactions**: Enable users to redeem wallet points during transactions, providing them with a seamless way to utilize their loyalty rewards.
* **Crediting Points to User Wallets**: Add points to the user wallet as and when needed.

This API is essential for businesses managing customer loyalty programs. It allows you to update loyalty balances based on user actions or rewards instantly. For example, if a user earns 100 loyalty points after a purchase, the API instantly credits the points to their wallet. Later, if the user returns the item, the API can debit the corresponding points, ensuring the loyalty balance remains accurate. By leveraging this API, businesses can enhance user experience and build trust through accurate and immediate loyalty point transactions.

### Example Use Case

Consider an example user case where a user makes a purchase worth $200 and has a balance of 200 points in their wallet. You can use the Debit/Credit Wallet API to redeem wallet points during this transaction as follows:

1. **Determine Redemption Details**: With a conversion rate of 1 point = $1, this redemption translates into a $50 discount.

2. **Calculate Final Payable Amount**: Apply the $50 discount to the purchase amount of $200. The user’s final payable amount is $150.

3. **Invoke the Debit API**: Invoke the Debit API to deduct the redeemed points from the user's wallet after completing the transaction successfully. Pass the transaction details, such as `walletId`, `identity`, and `points` to be debited (in this case, 50) in the API request.

4. **Update Wallet Ledger**: Update the wallet balance to **150 points**, ensuring the user wallet accurately reflects the redeemed points.

> 📘 Example Credit Use Case
>
> This above example illustrates a scenario specific to the Debit API. Consider an example where the current user balance is 200 points and earns 50 points after a transaction. In this case, the Credit API is invoked to add points to the user wallet, updating the wallet balance to 250 points.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/creditDebit](https://in1.api.clevertap.com/1/promo/creditDebit)

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

POST

# Query Parameters

| Parameter  | Description                                                            | Required/Optional | Type   | Sample Value |
| ---------- | ---------------------------------------------------------------------- | ----------------- | ------ | ------------ |
| `identity` | Identifies the user associated with the wallet.                        | Required          | String | `KMN@123`    |
| `walletId` | Unique identifier assigned to each user wallet within the application. | Required          | String | `wallet_123` |

# Body Parameters

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Required/Optional
      </th>

      <th>
        Datatype
      </th>

      <th>
        Sample Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `description`
      </td>

      <td>
        Description of the transaction.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `Redemption for order`
      </td>
    </tr>

    <tr>
      <td>
        `points`
      </td>

      <td>
        Loyalty points value to be debited/credited. Only positive point values are accepted. Requests with negative points for `debit` or `credit` transactions are rejected.
      </td>

      <td>
        Required
      </td>

      <td>
        Numeric
      </td>

      <td>
        `100`
      </td>
    </tr>

    <tr>
      <td>
        `transactionType`
      </td>

      <td>
        Type of transaction. Valid values are:

        * DEBIT
        * CREDIT
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        `DEBIT`
      </td>
    </tr>

    <tr>
      <td>
        `bucketType`
      </td>

      <td>
        Specifies the bucket where points are credited, either `ACTIVE` or `PENDING`. This applies only to credit transactions. If no value is provided, points are credited to the `ACTIVE` bucket by default.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `PENDING` or ` ACTIVE`
      </td>
    </tr>

    <tr>
      <td>
        `activationDuration`
      </td>

      <td>
        Specifies the time duration before points move from the Pending bucket to the Active bucket. This applies only to credit transactions. If the value is set to zero, the credit is applied immediately.

        The format is space-separated as follows:

        * Xw Xd Xh Xm, where:
          * X = value
          * w = week
          * d = day
          * h = hour
          * m = minute
      </td>

      <td>
        Optional
      </td>

      <td>
        Number
      </td>

      <td>
        `2d 3h`
      </td>
    </tr>

    <tr>
      <td>
        `expiryDuration`
      </td>

      <td>
        Specifies the expiry duration for credits. If not provided, the system defaults to the expiry duration configured at the wallet level.

        The format is space-separated as follows:

        * Xw Xd Xh Xm Xy, where:
          * X = value
          * w = week
          * d = day
          * h = hour
          * m = minute
          * y = calendar year
      </td>

      <td>
        Optional
      </td>

      <td>
        Number
      </td>

      <td>
        `1w 2d`
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
        Optional
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
        The source channel for the transaction (e.g., Mobile, Web, POS). Applies to both `credit` and `debit` transactions created via API.
      </td>

      <td>
        Optional
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
        Identifier for the transaction location (e.g., store code). Applies to both `credit` and `debit` transactions created via API.
      </td>

      <td>
        Optional
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
        `saleAmount`
      </td>

      <td>
        Gross order value tied to the transaction. Applies to both `credit` and `debit` transactions.
      </td>

      <td>
        Optional
      </td>

      <td>
        Numeric
      </td>

      <td>
        `2499.00`
      </td>
    </tr>

    <tr>
      <td>
        `metadata`
      </td>

      <td>
        The key–value pairs to persist with the transaction. Applies to both `credit` and `debit` transactions.
      </td>

      <td>
        Optional
      </td>

      <td>
        Object
      </td>

      <td>
        `{ "cashierId": "C42", "campaign": "FESTIVE25" }`
      </td>
    </tr>
  </tbody>
</Table>

<Callout icon="📘" theme="info">
  #### Metadata Validation

  * You may include any number of key–value pairs.
  * Do not send an empty object (\{}) for metadata. If metadata is present but empty, the request fails with error code 777 and message _metadata key value can not be empty_.
  * All key–value pairs in metadata are saved with each credit/debit transaction in the wallet transaction store.
</Callout>

<Callout icon="📘" theme="info">
  #### Debit Transactions Validation

  If debit transactions exceed available active points, they are not processed to prevent overdrafts in the wallet.
</Callout>

# Sample Request

Here is an example request to the Debit/Credit Wallet API, including the headers needed to authenticate the request:

```curl cURL
curl -X POST "https://in1.api.clevertap.com/1/promo/creditDebit?identity=krishna123&walletId=1963" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE \
-H "Content-Type: application/json" 
```

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the _Settings_ > _Project_ page on the CleverTap dashboard.

<Image align="center" alt="Find CleverTap Project Details" border={true} caption="Find CleverTap Project Details" src="https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png" />

The following are the request payloads for debit/credit transactions:

```json JSON for Debit Transactions
{
   "description": "Reward points for purchase",
   "points": 100,
   "transactionType": "DEBIT",
   "orderId": "order123",
   "saleChannel": "POS",
   "saleAmount": 1000,
   "locationId": "BAN123",
   "metadata":
    {
     "RuleId": "123",
     "vouchercode": "oomco123",
     "brand": "amazon"
   }
}
```
```json JSON for Credit Transactions
 {
   "description": "Reward points for purchase",
   "points": 100,
   "transactionType": "CREDIT",
   "bucketType": "ACTIVE",
   "expiryDuration": "1d 10m",
   "activationDuration": "3d",
   "orderId": "order123",
   "saleChannel": "POS",
   "saleAmount": 1000,
   "locationId": "BAN123",
   "metadata":
    {
     "RuleId": "123",
     "vouchercode": "oomco123",
     "brand": "amazon"
   }
}
```

# Sample Response

The sample response returns the updated wallet balance and transaction details in the following format:

```json
{
    "status": "success",
    "record": {
        "saleAmount": 1000,
        "walletId": 1963,
        "metadata": {
            "RuleId": "123",
            "brand": "amazon",
            "vouchercode": "oomco123"
        },
        "activePoints": 120.46,
        "orderId": "order123",
        "saleChannel": "POS",
        "locationId": "BAN123",
        "description": "Reward points for purchase",
        "tnxId": "wl_EjKlltmdYqrJYDiRaRcGv",
        "points": 100
    }
}
```

On a successful API call, the system debits or credits the specified points from the user's wallet based on the `transactionType` in the request body and returns a response indicating the updated wallet balance and transaction details.

> 📘 Note
>
> Response fields are illustrative. The `saleChannel`, `locationId`, `saleAmount`, `metadata` response fields affect what is stored, not necessarily what is returned. If your implementation returns any of these fields, document them in your environment’s response schema.

# Errors

To know the errors applicable to the Debit/Credit Wallet API, refer to [API Error Cases](https://staging.docs.dev.clevertap.net/docs/api-error-cases#wallet-api-errors).

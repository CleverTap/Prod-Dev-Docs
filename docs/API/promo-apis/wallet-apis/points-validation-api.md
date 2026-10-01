---
title: Points Validation API
excerpt: >-
  Learn how to use the Points Validation API to check whether a loyalty points
  redemption is allowed and calculate the points discount for your customers.
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

The Points Validation API enables you to verify whether a user's loyalty points redemption is allowed for a given cart and calculate the points discount based on the configured points ratio. Validating the redemption against the wallet's configured rules ensures accurate discounts without building the validation logic in your application.

The API evaluates the redemption rule configured in the loyalty wallet, applies the applicable points-to-currency ratio, calculates the discount amount, and returns a single result per call. You can integrate this API at relevant touchpoints within the application or website.

This API validates and calculates only. It does not move points. To deduct points from the wallet ledger, call the Debit/Credit Wallet API separately with the `pointsToDebit` value returned in the response.

You can use this API in the following cases:

- When the user enters the number of points to redeem at checkout.
- When the user modifies the cart after entering the points to redeem.
- When you need the points discount for the entered points, before debiting them.
- At any other custom checkpoint in the user journey where points redemption validation is required, such as a cart review step, a payment-options screen, or a final order-confirmation step, before the Debit call.

<Callout icon="📘" theme="info">
  #### Integration is Optional

  You can continue to use the Debit/Credit Wallet API on its own and manage redemption validation in your application. This API is also optional for direct points redemption use cases, such as buying a reward with points, even when you use it for the checkout flow. Choose the touchpoints where CleverTap validates the redemption for you.
</Callout>

# Prerequisites

Before you call this API, ensure the following:

- A redemption rule is saved on the loyalty wallet. Wallets created before this feature have no rule by default. Save one from the wallet edit flow, or the request fails.
- Any cart or product property referenced by the wallet's rules is included in the request payload.

# `Base URL`

The following is a sample base URL for the account in the India region:

```http
https://in1.api.clevertap.com/1/promo/wallets/points/validate?walletId=5167&identity=User1
```

For region-specific endpoints, refer to [Region](doc:common-api-components#region).

# HTTP Method

POST

# Headers

For more information about API headers, refer to [Headers](doc:common-api-components#headers).

# Path Parameters

The following table lists the path parameters:

| Parameter  | Description                                                                               | Required/Optional | Type    | Example Value |
| ---------- | ----------------------------------------------------------------------------------------- | ----------------- | ------- | ------------- |
| `identity` | The unique identifier for the user. Accepts a phone number, email, or CleverTap identity. | Required          | String  | user123       |
| `walletId` | The unique identifier of the loyalty wallet from which points are redeemed.               | Required          | Integer | 101           |

# Body Parameters

The request body accepts a `pointsRequested` field and an `order `object. The `items` array is nested inside the order object.

The following table lists the top-level body parameters:

| Parameter         | Description                                                                              | Required/Optional | Type    | Example Value |
| ----------------- | ---------------------------------------------------------------------------------------- | ----------------- | ------- | ------------- |
| `pointsRequested` | The number of points the user wants to redeem.                                           | Required          | Numeric | 100           |
| `order`           | Object containing order and item details. For more information, refer to `order` Object. | Required          | Object  |               |

## `order` Object

The `order` object provides information about the overall order. If the `order `object is not included, the API cannot validate the redemption effectively, as qualification and discount calculations depend on these order details.

The following table lists the order object parameters:

| Parameter      | Description                                                                                        | Required/Optional | Type             | Example Value                |
| -------------- | -------------------------------------------------------------------------------------------------- | ----------------- | ---------------- | ---------------------------- |
| `saleAmount`   | Sum of the selling price of all items, calculated as: `unitPrice` × `quantity`                     | Required          | Numeric          | 300                          |
| `saleChannel`  | Source through which the sale is made.                                                             | Optional          | String           | online                       |
| `location`     | Location where the sale is made.                                                                   | Optional          | String           | India                        |
| `locationId`   | Unique ID assigned to the location.                                                                | Optional          | String           | IND                          |
| `paymentMode`  | Mode of payment used for the transaction. For example, credit_card, debit_card, wallet, and so on. | Optional          | String           | wallet                       |
| `cartQuantity` | Total quantity of items in the cart.                                                               | Optional          | Numeric          | 3                            |
| `items`        | Array of item objects nested inside order. For more information, refer to `items` Array.           | Optional          | array of objects | \-                           |
| `metadata`     | Custom key-value pairs for cart-level qualification and ratio overrides.                           | Optional          | object           | {"customerType": "existing"} |

<Callout icon="📘" theme="info">
  ### Conditionally Mandatory Fields

  Only `pointsRequested` and `saleAmount` are mandatory in the schema. Any other field becomes mandatory when the wallet redemption rule references it. If a rule is configured on a property and the request does not include that property, the API returns a missing-key error.
</Callout>

# `items` Array&#x20;

The `items` array is nested inside the `order` object and provides details about individual items in the cart. The entire array is optional. An individual key becomes mandatory only when the product qualification rule or a product-based ratio override of the wallet is configured on that key. The API evaluates those rules against each item in the array.

The following table lists the `items` object parameters:

| Parameter     | Description                                                                 | Required/Optional | Type    | Example Value         |
| ------------- | --------------------------------------------------------------------------- | ----------------- | ------- | --------------------- |
| `productId`   | Unique identifier for the product in the application.                       | Optional          | String  | abc_124               |
| `sku`         | Stock Keeping Unit (SKU) assigned to the product.                           | Optional          | String  | 124                   |
| `itemName`    | Name of the product.                                                        | Optional          | String  | Slim Fit Cotton Shirt |
| `category`    | Category of the product.                                                    | Optional          | String  | shirts                |
| `subCategory` | Subcategory of the product.                                                 | Optional          | String  | cotton shirts         |
| `brand`       | Brand of the product.                                                       | Optional          | String  | Levis                 |
| `unitPrice`   | Selling price of the product.                                               | Optional          | Numeric | 80                    |
| `mrp`         | Original MRP of the product.                                                | Optional          | Numeric | 80                    |
| `quantity`    | Quantity of the product in the cart.                                        | Optional          | Numeric | 2                     |
| `metadata`    | Custom key-value pairs for product-level qualification and ratio overrides. | Optional          | Object  | {"fabric": "cotton"}  |

<Callout icon="📘" theme="info">
  ### Empty `items`  Array

  If a product qualification rule is configured and `order.items` is empty, the rule is not satisfied for either Any of or All of mode, and the redemption is rejected.
</Callout>

## `metadata` Object

The `metadata` object allows you to include additional custom key-value pairs for cart and product qualification. It can be passed within both the `order` and `items` objects. The key must match the property name configured in the dashboard.

The `paymentMode` and `cartQuantity` fields are the core properties of the `order` object, not `metadata` properties.

The following table lists the metadata object parameters:

| Parameter | Description                                                          | Required/Optional | Type   | Example Value |
| --------- | -------------------------------------------------------------------- | ----------------- | ------ | ------------- |
| `key`     | Cart or product property name (for example, customerType, operator). | Optional          | String | customerType  |
| `value`   | Value associated with the cart or product property.                  | Optional          | String | existing      |

# Example Request

The following is a sample request to the Points Validation API, showing the headers needed to authenticate the request:

```curl
curl --location --request POST \
'https://in1.api.clevertap.com/1/promo/wallets/points/validate?walletId=5167&identity=User1' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw '{
  "pointsRequested": 100,
  "order": {
    "saleAmount": 1200,
    "saleChannel": "online",
    "location": "India",
    "locationId": "IND",
    "paymentMode": "wallet",
    "cartQuantity": 3,
    "metadata": {
      "customerType": "existing"
    },
    "items": [
      {
        "productId": "abc_124",
        "sku": "124",
        "itemName": "Slim Fit Cotton Shirt",
        "category": "shirts",
        "subCategory": "cotton shirts",
        "brand": "levis",
        "unitPrice": 400,
        "mrp": 400,
        "quantity": 3,
        "metadata": {
          "fabric": "cotton"
        }
      }
    ]
  }
}'
```

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to _Settings_ > _Project_.

# Example Response

The API returns two different response shapes, and the distinction affects how you parse the result.

A redemption that passes validation and a redemption rejected by a business rule both return status as success in the same envelope. A business rule rejection returns `isApplicable` as `false` with a code and message, and includes `requestId`.

<Callout icon="📘" theme="info">
  ### `requestId`  not Returned on Every Response

  A malformed request returns an error with `status` as fail, which does not include the `requestId `field.
</Callout>

## Redemption Allowed

```json
{
  "status": "success",
  "records": [
    {
      "isApplicable": true,
      "message": "",
      "pointsRequested": 100,
      "pointsToDebit": 100,
      "adjusted": false,
      "maxPointsUnlimited": true,
      "pointsDiscount": 100.00,
      "currency": "USD",
      "pointRatio": {
        "type": "default",
        "overrideName": "default",
        "value": 1
      },
      "requestId": "req_8827123"
    }
  ]
}
```

## Redemption Rejected by Business Rule

```json
{
  "status": "success",
  "records": [
    {
      "isApplicable": false,
      "message": "Insufficient balance, you only have 80 points available",
      "pointsRequested": 100,
      "adjusted": false,
      "pointsDiscount": 0,
      "currency": "USD",
      "pointRatio": {},
      "requestId": "req_8827124"
    }
  ]
}
```

## Maximum Points Discount Exceeded

When the calculated discount exceeds the configured cap, the response returns both the currency cap and its points equivalent, so you do not need to reverse-calculate the redeemable points.

```json
{
  "status": "success",
  "records": [
    {
      "isApplicable": false,
      "message": "You can redeem up to 10 points ($1.00) on this order.",
      "pointsRequested": 100,
      "maxPointsDiscount": 10,
      "maxPointsToDebit": 1.00,
      "pointsDiscount": 0,
      "currency": "USD",
      "pointRatio": {},
      "requestId": "req_8827125"
    }
  ]
}
```

## Invalid Request

A missing or incorrectly typed parameter fails during schema validation, before any rule is evaluated. This response does not include a `records` array.

```json
{
  "status": "fail",
  "error": "saleAmount is required",
  "code": 752
}
```

# Response Parameters

| Parameter                 | Description                                                                                         | Type    | Behavior                                                                                                                                                                          |
| ------------------------- | --------------------------------------------------------------------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `isApplicable`            | Indicates if the requested redemption is valid and can proceed.                                     | Boolean | Returns `true` when all qualification and cap checks pass, with or without adjustment. Returns `false` for any validation failure.                                                |
| `code`                    | Numeric code for the business rule that rejected the redemption.                                    | Number  | Returned only when `isApplicable` is false, Returned in `records` when `isApplicable` is false, and at the top level when `status` is fail.                                       |
| `message`                 | Validation failure message.                                                                         | String  | Returned only when `isApplicable` is false. Returns an empty string otherwise.                                                                                                    |
| `pointsRequested`         | The number of points requested in the payload.                                                      | Number  | Always returned exactly as received.                                                                                                                                              |
| `pointsToDebit`           | The number of points to pass to the Debit/Credit Wallet API.                                        | Number  | Returned only when `isApplicable` is true.                                                                                                                                        |
| `pointsDiscount`          | The final discount value in currency applied to the order.                                          | Number  | Returned only when `isApplicable` is true, rounded to two decimal places, or equal to `saleAmount` in the clamp case. Returns 0 when `isApplicable` is false.                     |
| `adjusted`                | Indicates if `pointsToDebit` differs from `pointsRequested`.                                        | Boolean | Returns true when the two values differ, false otherwise.                                                                                                                         |
| `maxPointsDiscount`       | The maximum discount value in currency allowed on this order.                                       | Number  | Returned only when the maximum points check fails. Absent in all other cases.                                                                                                     |
| `maxPointsToDebit`        | The maximum number of points redeemable on this order, reverse-calculated from `maxPointsDiscount`. | Number  | Returned only when the maximum points check fails. Absent in all other cases.                                                                                                     |
| `maxPointsUnlimited`      | Indicates if the maximum points discount is configured as unlimited.                                | Boolean | Returns true when the Unlimited unit is set, false otherwise.                                                                                                                     |
| `currency`                | ISO 4217 currency code of the resolved ratio.                                                       | String  | Returns the currency configured on the applied ratio.                                                                                                                             |
| `pointRatio`              | Details of the ratio, default or override, resolved for this request.                               | Object  | Populated when `isApplicable` is true. Returns an empty object when false.                                                                                                        |
| `pointRatio.type`         | Whether the applied ratio was the wallet default or a matched override.                             | String  | Returns either default or override.                                                                                                                                               |
| `pointRatio.overrideName` | The marketer-assigned label for the applied ratio setting.                                          | String  | Returns default when `type` is default, or the configured override name, such as Override 1, when `type` is override.                                                             |
| `pointRatio.value`        | The currency value of one point in the resolved ratio.                                              | Number  | Returns the exact configured value, at full precision.                                                                                                                            |
| `requestId`               | Unique identifier for this validation request.                                                      | String  | Returned on all responses in the success envelope, including business rule rejections. Not returned when status is fail. It is used for logging, audit trail, and reconciliation. |

<Callout icon="📘" theme="info">
  #### Always Debit `pointsToDebit`

  An `adjusted` value if true indicates that the points to debit differ from the points requested because a rounding, `saleAmount`, or balance adjustment was applied. Always pass `pointsToDebit` to the Debit/Credit Wallet API, not `pointsRequested`.
</Callout>

## Points Ratio Resolution

Overrides are checked in the rank order the marketer sets. The first override whose conditions fully match is applied, and evaluation stops there. If no override matches, the wallet's default ratio applies. An override whose key is missing from the payload simply does not match; this is not an error.

Open-ended operators can make more than one override match for the same cart. For example, `cart_value` >= 1000 and `cart_value` >= 1500 is true for a cart of 1800. Rank order decides which applies.&#x20;

Discount is calculated as follows:

`calculatedValue` = `pointsRequested`  x `ratio.value`&#x20;

This value is only used for comparison. The API checks it against the cap and `saleAmount` before rounding, so the final discount is never derived from a rounded figure.

The API then runs the following steps in order. The order matters because the cap check and the saleAmount comparison both happen before any rounding.

1. Calculate the comparison value. Multiply the requested points by the resolved ratio. This is not the final discount.
2. Check the maximum points discount cap. For Flat and Percentage, if the comparison value exceeds the cap, the redemption is rejected with code 759, and the response returns `maxPointsDiscount` and `maxPointsToDebit`. Unlimited has no cap, so it always passes this step.
3. Compare against `saleAmount`. The points discount is the lower of the comparison value and `saleAmount`.
4. Derive the final points discount. If the comparison value is lower, round it half-up to the default decimal cap for the points discount, which is two decimal places. If `saleAmount` is lower, use `saleAmount` exactly as sent, with no rounding.
5. Reverse-calculate the points to debit. Divide the final points discount by the ratio value, then truncate to the decimal-place setting of the wallet.
6. Apply the balance safety check. If the points to debit exceed the available balance, cap them at the balance. This is a silent adjustment, not an error.

### Examples

All four examples use a ratio of 0.00005 USD per point and a wallet decimal-place setting of 2.

- **Points discount decimal cap:** The default number of decimal places to which the calculated points discount is rounded.&#x20;
- **Wallet decimal-place setting:** The points-precision setting of the wallet, used to truncate the points to debit (step 5).

#### Example 1

Rounding applied. 651,300 points requested, saleAmount 50.00, available balance 700,000, maximum points discount Unlimited.

<Table>
  <thead>
    <tr>
      <th>
        Step
      </th>

      <th>
        Result
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1. Comparison value
      </td>

      <td>
        651,300 × 0.00005 = 32.565
      </td>
    </tr>

    <tr>
      <td>
        2. Cap check
      </td>

      <td>
        Unlimited, so no cap to breach
      </td>
    </tr>

    <tr>
      <td>
        3. Compare against `saleAmount`
      </td>

      <td>
        32.565 is lower than 50.00
      </td>
    </tr>

    <tr>
      <td>
        4. Final points discount
      </td>

      <td>
        round(32.565, 2) = 32.57, where 2 is the points discount decimal cap
      </td>
    </tr>

    <tr>
      <td>
        5. Points to debit
      </td>

      <td>
        truncate(32.57 ÷ 0.00005, 2) = 651,400, where 2 is the wallet decimal-place setting
      </td>
    </tr>

    <tr>
      <td>
        6. Balance check
      </td>

      <td>
        651,400 is within 700,000, so no cap
      </td>
    </tr>
  </tbody>
</Table>

The response returns pointsDiscount 32.57, pointsToDebit 651,400, and adjusted true. The points to be debited exceed the requested points because the discount was rounded up.

#### Example 2

Comparison value exceeds saleAmount. 1,200,000 points requested, saleAmount 50.00, available balance 1,200,000, maximum points discount Unlimited.

<Table>
  <thead>
    <tr>
      <th>
        Step
      </th>

      <th>
        Result
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1. Comparison value
      </td>

      <td>
        1,200,000 × 0.00005 = 60.00
      </td>
    </tr>

    <tr>
      <td>
        2. Cap check
      </td>

      <td>
        Unlimited, so no cap to breach
      </td>
    </tr>

    <tr>
      <td>
        3. Compare against `saleAmount`
      </td>

      <td>
        60.00 is higher than 50.00
      </td>
    </tr>

    <tr>
      <td>
        4. Final points discount
      </td>

      <td>
        50.00, used as sent with no rounding
      </td>
    </tr>

    <tr>
      <td>
        5. Points to debit
      </td>

      <td>
        truncate(50.00 ÷ 0.00005, 2) = 1,000,000
      </td>
    </tr>

    <tr>
      <td>
        6. Balance check
      </td>

      <td>
        1,000,000 is within 1,200,000, so no cap
      </td>
    </tr>
  </tbody>
</Table>

The response returns pointsDiscount 50.00, pointsToDebit 1,000,000, and adjusted true. Fewer points are debited than requested, because the discount cannot exceed the order value.

#### Example 3

Maximum points discount exceeded. 651,300 points requested, saleAmount 50.00, maximum points discount 20.00 Flat.

<Table>
  <thead>
    <tr>
      <th>
        Step
      </th>

      <th>
        Result
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1. Comparison value
      </td>

      <td>
        651,300 × 0.00005 = 32.565
      </td>
    </tr>

    <tr>
      <td>
        2. Cap check
      </td>

      <td>
        32.565 exceeds the 20.00 cap, so the redemption is rejected
      </td>
    </tr>
  </tbody>
</Table>

The API stops at step 2 and returns `isApplicable` false with code 759, `maxPointsDiscount` 20.00, and `maxPointsToDebit` 400,000. Steps 3 to 6 do not run, so no discount is calculated.

#### Example 4

Balance safety check applied. 651,300 points requested, saleAmount 50.00, available balance 651,350, maximum points discount Unlimited.

<Table>
  <thead>
    <tr>
      <th>
        Step
      </th>

      <th>
        Result
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1. Comparison value
      </td>

      <td>
        651,300 × 0.00005 = 32.565
      </td>
    </tr>

    <tr>
      <td>
        2. Cap check
      </td>

      <td>
        Unlimited, so no cap to breach
      </td>
    </tr>

    <tr>
      <td>
        3. Compare against `saleAmount`
      </td>

      <td>
        32.565 is lower than 50.00
      </td>
    </tr>

    <tr>
      <td>
        4. Final points discount
      </td>

      <td>
        round(32.565, 2) = 32.57
      </td>
    </tr>

    <tr>
      <td>
        5. Points to debit
      </td>

      <td>
        truncate(32.57 ÷ 0.00005, 2) = 651,400
      </td>
    </tr>

    <tr>
      <td>
        6. Balance check
      </td>

      <td>
        651,400 exceeds 651,350, so the points are capped at 651,350
      </td>
    </tr>
  </tbody>
</Table>

The response returns `pointsDiscount` 32.57, `pointsToDebit` 651,350, and adjusted true. The user receives the full discount, and the small reconciliation gap falls in the user's favor.

The cap itself is derived per unit:

| Unit       | Cap                                                                 |
| ---------- | ------------------------------------------------------------------- |
| Flat       | The configured value                                                |
| Percentage | Percentage of `saleAmount` or  `pointsDiscount`, whichever is lower |
| Unlimited  | `saleAmount`                                                        |

## Points to Debit

`pointsToDebit` is always reverse-calculated from the final `pointsDiscount`, in both the rounded and the clamped branch.

`maxDecimalPlace` is the wallet's points-precision setting, configured per wallet in the dashboard. It is not a request field and not a global constant.<br />

Truncation, rather than rounding, keeps the reverse calculation from inflating the figure at the wallet's points precision. Because `pointsDiscount` can round up, or can equal saleAmount directly, `pointsToDebit` can be higher than pointsRequested. That is expected, and adjusted is returned as true.<br />

If `pointsToDebit` exceeds `activePoints`, it is capped to `activePoints` and `adjusted` is true. This is a second balance check, the earlier check against `pointsRequested` is not sufficient on its own, because `pointsToDebit` is not known until after the calculation.&#x20;

## Validation Scenarios

The API validates the request in a fixed sequence and behaves in one of three ways at each step. It rejects the request, continues to the next step, or accepts the redemption with a silent adjustment. The following table lists the behavior for each case, in the sequence in which the API evaluates them.&#x20;

For the message and code returned on a rejection, refer to the Errors section below.

| Case                                                                              | Behavior               | Details                                                                                                                                                                                                                         |
| --------------------------------------------------------------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `identity` is not passed                                                          | Reject                 | The request is rejected with an HTTP 400 error before any rule is evaluated.                                                                                                                                                    |
| `walletId` is not passed                                                          | Reject                 | The request is rejected with an HTTP 400 error before any rule is evaluated.                                                                                                                                                    |
| Incorrect `walletId` is passed                                                    | Reject                 | Returned as a business rule rejection with `isApplicable` as false.                                                                                                                                                             |
| Data type mismatch on any key                                                     | Reject                 | Request fails at schema validation.                                                                                                                                                                                             |
| `pointsRequested` key or value is not passed                                      | Reject                 | Request fails at schema validation.                                                                                                                                                                                             |
| `pointsRequested` is zero or negative                                             | Reject                 | Request fails at schema validation.                                                                                                                                                                                             |
| `saleAmount` is not passed                                                        | Reject                 | Request fails at schema validation.                                                                                                                                                                                             |
| `saleAmount` is zero or negative                                                  | Reject                 | Request fails at schema validation.                                                                                                                                                                                             |
| Wallet exists but has no redemption rule configured                               | Reject                 | Fails before any rule is evaluated. The wallet has nothing to validate against.                                                                                                                                                 |
| Points requested exceed the active balance                                        | Reject                 | Fails at the validation check. The user does not hold enough points for this specific request.                                                                                                                                  |
| Active balance is below the configured minimum points balance                     | Reject                 | Fails at the validation check. This is a standing-balance floor, independent of the points requested in this transaction.                                                                                                       |
| Cart qualification: key configured but not present in the payload                 | Reject                 | A missing key is treated as not satisfied. With AND, the error returns on the first failing check. With OR, all keys are evaluated before the error returns.                                                                    |
| Cart qualification: key is present, but the value does not match                  | Reject                 | The cart payload does not satisfy the configured cart qualification rule. With AND, the error returns on the first failing check. With OR, all keys are evaluated before the error returns.                                     |
| Product qualification: key configured but not present in the payload              | Reject                 | A missing key on an item is treated as not satisfied for that item, which feeds the Any of or All of outcome. With AND, the error returns on the first failing check. With OR, all keys are evaluated before the error returns. |
| Product qualification: key is present but the value does not match                | Reject                 | The item payload is not satisfied for that item, which feeds the Any of or All of outcome. With AND, the error returns on the first failing check. With OR, all keys are evaluated before the error returns.                    |
| Multiple qualification checks fail at the same time                               | Reject                 | Only the first failing check in the priority order is returned. The API fails fast and does not aggregate failures.                                                                                                             |
| Override conditions configured, but the key or value is missing or does not match | Continue               | Not an error. That override does not match, and evaluation proceeds to the next-ranked override.                                                                                                                                |
| No override matches any configured condition                                      | Continue               | Falls back to the default ratio and proceeds normally.                                                                                                                                                                          |
| Exactly one override matches                                                      | Continue               | That override's ratio is used. Lower-ranked overrides are not evaluated.                                                                                                                                                        |
| Two or more overrides would independently match                                   | Continue               | The first match in rank order wins and evaluation stops there.                                                                                                                                                                  |
| Maximum points discount is exceeded for Flat or Percentage                        | Reject                 | The marketer-configured budget is violated. Both the currency cap and its points equivalent are reverse-calculated and returned.                                                                                                |
| Maximum points discount is Unlimited                                              | Continue               | No violation is possible on this check. Flows directly into the `saleAmount` comparison.                                                                                                                                        |
| Calculated discount exceeds `saleAmount`                                          | Accept with adjustment | Silent clamp, no error. The discount is capped at `saleAmount`, the points are reverse-calculated, and `adjusted` returns true.                                                                                                 |
| Reverse-calculated points to debit exceed the active balance                      | Accept with adjustment | Silent cap, no error. The points to debit are set to the active balance and `adjusted` returns true.                                                                                                                            |
| Calculated discount is within `saleAmount` and within the configured cap          | Accept                 | Success with no adjustment. The points to debit equal the points requested.                                                                                                                                                     |

# Errors

The following table lists the errors specific to the Points Validation API:

| Scenario                                                                                                | Message                                                                                                    |
| ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `pointsRequested` is missing, zero, negative, or a fractional value.                                    | pointsRequested must be a whole number                                                                     |
| Request schema validation failure, including a missing or invalid `saleAmount` or a data type mismatch. | Varies by field. For example, saleAmount is required.                                                      |
| The wallet exists but has no redemption rule configured.                                                | This wallet does not have redemption rules configured                                                      |
| The points requested exceed the user's active balance.                                                  | Insufficient balance, you only have {activePoints} points available                                        |
| The active balance is below the configured minimum points balance.                                      | You need at least {X} active points to redeem                                                              |
| The calculated discount exceeds the configured maximum points discount per order.                       | You can redeem up to {maxPointsToDebit} points ({currency}{maxPointsDiscount}) on this order               |
| A key configured on a qualification rule is missing from the payload.                                   | {property} is required. The API rejects the request on the first failure and returns only one missing key. |

For errors applicable to all CleverTap API requests, refer to [API Error Cases](doc:api-error-cases).

## Qualification Failure Messages

Cart and product qualification failures return one of two formats, depending on how many conditions failed.

| Number of Failing Conditions | Format                                                   | Example                                   |
| ---------------------------- | -------------------------------------------------------- | ----------------------------------------- |
| One                          | `{property} must {operator phrase} {configured value}`   | Enter a whole number                      |
| More than one                | enter a value with up to {decimal_places} decimal places | Enter a value with up to 2 decimal places |

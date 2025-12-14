---
title: Redeem/Revert
excerpt: >-
  Learn how to use the Redeem/Revert Coupon API to update the coupon code
  status, ensuring accurate application and reversals during transactions.
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

The Redeem/Revert Coupon API allows you to manage coupon code redemption and reversal during transactions. This API ensures accurate tracking of coupon usage by updating their status based on the transaction outcome, whether successful or canceled.

You can integrate this API at critical touchpoints, such as payment processing or order cancelation flows. Upon execution, CleverTap processes the coupon redemption or reversal request and provides a response indicating the updated status of the coupon code for a particular user.

You can use this API in the following cases:

- A user completes a transaction, and the coupon code needs to be redeemed.
- A user cancels an order, and the redeemed coupon needs to be reverted for future use.
- Payment failure after the coupon code is redeemed.
- Any other custom checkpoints in the user journey that require coupon redemption or reversal during transaction management.  

# Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/1/promo/coupons/apply>

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

POST

# Query Parameters

| Parameter  | Description                                                                                                                                                                                                                                             | Required/Optional | Type   | Sample Value       |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | ------ | :----------------- |
| `identity` | Unique identifier for the user. It is mandatory for campaign user coupon validation to determine the source of the coupon issued. If the `identity` is not provided, API returns an error message indicating that identity is mandatory for redemption. | Required          | String | `user0123`         |
| `type`     | Specifies the action to be taken. Use `redeem` to apply a coupon or `revert` to cancel a redeemed coupon.                                                                                                                                               | Required          | String | `redeem`, `revert` |

> 📘 Note
> 
> - If the `identity` is not provided for non logged-in users, the API returns an error message asking the user to log in to redeem the coupon code.
> - In the case of invalid or missing `type` parameter, the API returns an error message indicating the `type` query parameter must be either `redeem` or `revert`.

# Body Parameters

The body parameters define the structure and data required to redeem a coupon. These parameters include key objects that provide information about the coupon, the associated order, items, and other contextual details necessary for the API to process the redemption request.

## `coupon` Object

The `coupon` object contains the essential details to validate, redeem, or revert a coupon. Depending on the type of coupon, it includes either a coupon ID or coupon code. This object is used to process and apply the coupon in different redemption scenarios.

If the coupon details are not provided, the API returns an error message indicating that coupon details are mandatory for redemption.

| Parameter    | Description                                | Required/Optional | Data Type | Sample Value |
| ------------ | :----------------------------------------- | ----------------- | --------- | ------------ |
| `couponId`   | Coupon ID generated against a coupon code. | Optional          | String    | `coupon_123` |
| `couponCode` | Unique coupon code to be validated.        | Required          | String    | `DIWALI100`  |

> 📘 Coupon ID and Coupon Code Handling
> 
> The following are the conditions and their system handling for the `couponId` and `couponCode`:
> 
> - **For Fixed Code Coupons**: For fixed codes, you can provide either a `couponCode` or a `couponID`. If both parameters are provided, the system validates the coupon using either one, as long as one of them is correct.
> - **For Bulk Code Coupons**: For bulk codes, you must provide the coupon code. The coupon ID and coupon code can have the same value, but the system only validates the coupon based on the provided `couponCode`. If the `couponCode` is invalid or not found, the system returns an error.

## `order` Object

The `order` object provides information about the overall order and is a mandatory parameter. If the `order` object is not included, the API cannot validate the coupon effectively, as the coupon's applicability and calculations depend on these order details.

| Parameter       | Description                                                                                                                                                                                                                                                                                                                               | Required/Optional | Data Type | Sample Value  |
| --------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | --------- | ------------- |
| `orderId`       | Unique identifier for the order being placed. This ID is used to track transactions and associate them with a specific purchase. For example, if a user is buying a shirt, the `orderId` can be `abcd12345`.                                                                                                                              | Required          | String    | `abcd12345`   |
| `saleMrpTotal`  | Sum of the original price of all items, calculated as `MRP` x `quantity`.                                                                                                                                                                                                                                                                 | Optional          | Numeric   | `500`         |
| `saleAmount`    | Sum of the selling price of all items, calculated as`unitPrice` x `quantity`                                                                                                                                                                                                                                                              | Required          | Numeric   | `300`         |
| `saleChannel`   | Source through which the sale is made.                                                                                                                                                                                                                                                                                                    | Optional          | String    | `online`      |
| `location`      | Location of the sale made.                                                                                                                                                                                                                                                                                                                | Optional          | String    | `India`       |
| `locationId`    | Unique ID assigned to a location.                                                                                                                                                                                                                                                                                                         | Optional          | String    | `IND`         |
| `orderShipping` | Sum of the shipping charges at an order level.                                                                                                                                                                                                                                                                                            | Optional          | Numeric   | `30`          |
| `orderStatus`   | <p>Status of the transaction on the user's application, i.e., `pending`, `completed`, or `reverted`.</p><li>**Pending**: The transaction is initiated but not yet completed.</li><li>**Completed**: The transaction is successfully completed.</li><li>**Reverted**: The transaction is rolled back, and the coupon redemption is undone. | Required          | String    | `pending`     |
| `paymentMode`   | Mode of payment used for the transaction. For example, `credit_card`, `debit_card`, `net_banking` and so on.                                                                                                                                                                                                                              | Optional          | String    | `credit_card` |
| `cartQuantity`  | Total quantity of items in the cart.                                                                                                                                                                                                                                                                                                      | Optional          | Numeric   | `4`           |

## `item` Object

The `item` object provides details about individual items in the cart and is a mandatory parameter for coupon redemption. If the item object is not included, the API cannot process the redemption correctly, as it relies on item-specific information such as `productId`, `unitPrice`, and `quantity` to redeem coupons.

| Parameter     | Description                                                           | Required/Optional | Data Type | Sample Value    |
| ------------- | --------------------------------------------------------------------- | ----------------- | --------- | --------------- |
| `productId`   | Product identification ID from your application.                      | Required          | String    | `abc_124`       |
| `sku`         | Assigned Stock Keeping Unit (SKU) to a product from your application. | Optional          | String    | `124`           |
| `itemName`    | Name of the product.                                                  | Optional          | String    | `Coke`          |
| `category`    | Name of the product category.                                         | Optional          | String    | `shirts`        |
| `subCategory` | Name of the product subcategory.                                      | Optional          | String    | `cotton shirts` |
| `brand`       | Name of the product brand.                                            | Optional          | String    | `superdry`      |
| `unitPrice`   | Selling price of the product.                                         | Required          | Numeric   | `80`            |
| `mrp`         | MRP of the product.                                                   | Optional          | Numeric   | `700`           |
| `quantity`    | Quantity of the product in the cart.                                  | Required          | Numeric   | `12`            |

## `metadata` Object

The `metadata` object allows you to include additional custom key-value pairs for advanced coupon validations.

| Parameter | Description                                                    | Required/Optional | Data Type | Sample Value |
| --------- | -------------------------------------------------------------- | ----------------- | --------- | ------------ |
| key       | Metadata property name (for example, color, size, supplier).   | Optional          | Object    | `size`       |
| value     | Metadata property value (for example, Red, Large, Supplier A). | Optional          | Object    | `large`      |

# Sample Request

Here is an example request to the Redeem/Revert Coupon API, including headers needed to authenticate the request:

```json Redeem cURL
curl POST "https://in1.api.clevertap.com/1/promo/coupons/apply?identity=krishna123&type=redeem" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" 
-data '{   
    "couponCode": "SALE100",
    "order": {
        "orderId": "order123349",
        "saleMrpTotal": 399,
        "saleAmount": 101,
        "saleChannel": "online",
        "location": "BLR",
        "locationId": "IND",
        "orderStatus": "pending",
        "items": [
        {
            "productId": "123456",
            "sku": "124",
            "itemName": "i1",
            "category": "electronics",
            "subCategory": "",
            "brand": "Zee5",
            "unitPrice":300,
            "mrp":300,
            "quantity": 3,
            "metadata": {
                "color": "blue",
                "newkey" : "k1"
            }
        },
        {
            "productId": "123456",
            "sku": "124",
            "itemName": "i1",
            "category": "electronics",
            "subCategory": "",
            "brand": "Zee5",
            "unitPrice":100,
            "mrp":100,
            "quantity": 1,
            "metadata": {
                "color": "blue",
                "newkey" : "k1"

            }
        }
    ],
        "metadata": {
            "customerType": "guests"
        }
    }
}'
```
```json Revert cURL
curl POST "https://in1.api.clevertap.com/1/promo/coupons/apply?identity=krishna123&type=revert" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" 
-data '{   
    "couponCode": "SALE100",
    "couponId" : 1695,
    "order": {
        "orderId": "order123349",
        "saleMrpTotal": 399,
        "saleAmount": 101,
        "saleChannel": "online",
        "location": "BLR",
        "locationId": "IND",
        "orderStatus": "pending",
        "items": [
        {
            "productId": "123456",
            "sku": "124",
            "itemName": "i1",
            "category": "electronics",
            "subCategory": "",
            "brand": "Zee5",
            "unitPrice":300,
            "mrp":300,
            "quantity": 3,
            "metadata": {
                "color": "blue",
                "newkey" : "k1"
            }
        },
        {
            "productId": "123456",
            "sku": "124",
            "itemName": "i1",
            "category": "electronics",
            "subCategory": "",
            "brand": "Zee5",
            "unitPrice":100,
            "mrp":100,
            "quantity": 1,
            "metadata": {
                "color": "blue",
                "newkey" : "k1"

            }
        }
    ],
        "metadata": {
            "customerType": "guests"
        }
    }
}'
```
```python Redeem Python

```
```python Revert Python

```
```
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

```json Redeem
{
    "status": "success",
    "records": [
        {
            "couponTimeframe": [
                {
                    "timeSlot": {
                        "startTime": "00:00AM",
                        "endTime": "11:59PM"
                    },
                    "validationDay": "All"
                }
            ],
            "couponName": "SALE100",
            "couponExpiry": 32494151843,
            "couponCodeId": 2255,
            "totalRedemptionUnlimited": true,
            "isApplicable": true,
            "description": "",
            "message": "",
            "couponExpiryNever": true,
            "termsAndConditions": [],
            "discountEffect": {
                "discountRate": 99,
                "rewardType": "cashback",
                "discountType": "entireOrder",
                "discountAppliedOn": "saleAmount",
                "discountValue": "absolute"
            },
            "totalRedemptionLeft": 0,
            "couponSavings": {
                "customPropertyDiscount": null,
                "orderDiscount": 0,
                "shippingDiscount": 0,
                "orderCashbackPoints": 0,
                "order": {
                    "saleAmount": 2300,
                    "metadata": {
                        "customerType": "premium",
                        "transactionId": "UPI123456789"
                    },
                    "saleMrpTotal": 2500,
                    "orderId": "order_pos_00e4",
                    "orderStatus": "pending",
                    "itemsTotalQuantity": 0,
                    "itemsMrpTotal": 0,
                    "saleChannel": "online",
                    "orderShipping": 100,
                    "locationId": "IND",
                    "location": "BLR",
                    "items": [],
                    "orderNetAmount": 2300,
                    "itemsSaleAmount": 0
                }
            },
            "bestCoupon": "",
            "identity": "j10393",
            "requestId": "",
            "redemptionActivationDate": 1756405800,
            "userRedemptionUnlimited": false,
            "redemptionStatus": true,
            "couponCode": "SALE100",
            "userRedemptionLeft": 1
        }
    ]
}
```
```json Revert
{
    "status": "success",
    "records": [
        {
            "couponTimeframe": null,
            "couponName": "",
            "couponExpiry": 0,
            "couponCodeId": 2255,
            "totalRedemptionUnlimited": false,
            "isApplicable": false,
            "description": "",
            "message": "",
            "couponExpiryNever": false,
            "termsAndConditions": null,
            "discountEffect": {
                "discountRate": 0,
                "rewardType": "cashback",
                "discountType": "",
                "discountAppliedOn": "",
                "discountValue": ""
            },
            "totalRedemptionLeft": 0,
            "couponSavings": {
                "customPropertyDiscount": null,
                "orderDiscount": 0,
                "shippingDiscount": 0,
                "orderCashbackPoints": 0,
                "order": {
                    "saleAmount": 0,
                    "metadata": null,
                    "saleMrpTotal": 0,
                    "orderId": "order_pos_00e4",
                    "orderStatus": "",
                    "itemsTotalQuantity": 0,
                    "itemsMrpTotal": 0,
                    "saleChannel": "",
                    "orderShipping": 0,
                    "locationId": "",
                    "location": "",
                    "items": null,
                    "orderNetAmount": 0,
                    "itemsSaleAmount": 0
                }
            },
            "bestCoupon": "",
            "identity": "j10393",
            "requestId": "",
            "redemptionActivationDate": 0,
            "userRedemptionUnlimited": false,
            "redemptionStatus": false,
            "couponCode": "SALE100",
            "userRedemptionLeft": 0
        }
    ]
}
```

# Understanding API Response

The Redeem/Revert Coupon API shares several behavior points with the Validate Coupon API. Refer to the [Validate Coupon API Behavior](doc:validate-coupon#api-behaviour) section for applicable rules, including:

- Identity validation
- Order and Item Object validation
- Optional parameters validation
- Metadata validation
- Coupon code or coupon ID validation
- Day-specific coupon availability
- Case sensitivity of coupon codes

Additional behaviors specific to the Redeem/Revert API are as follows:

| Scenario                                                                                                                                                                                                                                                                                                                                                                                                                                                              | API Behavior                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Redemption Validation**                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                           |
| Coupon redeemed for non-logged-in users                                                                                                                                                                                                                                                                                                                                                                                                                               | The API returns an error message, as coupons cannot be redeemed by non-logged-in users.                   |
| Coupon redemption against duplicate order ID                                                                                                                                                                                                                                                                                                                                                                                                                          | The API returns an error message if a coupon is redeemed against the same order ID as a normal coupon.    |
| **Reversion Validation**                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                           |
| Reverting a coupon with a mismatched order ID                                                                                                                                                                                                                                                                                                                                                                                                                         | The API returns an error message if a coupon is reverted without matching the original redeemed order ID. |
| <p>Stacking normal and `customProperty`coupons.</p><li>**Normal Coupon**: A coupon that applies discounts to predefined cart conditions, such as 10% off on the entire cart. For example, `entireOrder` `cartExcluding`, or `selectedItems`.</li><li>**Custom Property Coupon**: A coupon that applies discounts based on custom attributes like shipping charges or membership tiers, such as free shipping for premium members. For example, `customProperty`.</li> | Users can redeem one normal coupon with multiple `customProperty` coupons against the same order ID.      |
| Stacking normal coupons                                                                                                                                                                                                                                                                                                                                                                                                                                               | Users cannot redeem the same or different normal coupons multiple times against the same order ID.        |
| Stacking `customProperty` coupons                                                                                                                                                                                                                                                                                                                                                                                                                                     | Users cannot redeem the same `customProperty` coupon multiple times against the same order ID.            |
| Duplicate order IDs                                                                                                                                                                                                                                                                                                                                                                                                                                                   | The API validates duplicate order IDs based on the combination of coupon type and order ID.               |

# Errors

To know about the errors applicable to the Redeem/Revert Coupon API, refer to [API Error Cases](doc:api-error-cases#coupon-api-errors).
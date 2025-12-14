---
title: Validate Coupon
excerpt: >-
  Learn how to use the Validate Coupon API to verify coupon codes and enable a
  smooth checkout experience for your customers.
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

The Validate Coupon API enables you to verify the validity and applicability of a coupon code users enter during the checkout process. Verifying the coupon details with cart and product metadata ensures accurate discounts and a seamless shopping experience.

You can integrate this API at relevant touchpoints within the application or website. Upon successful validation, CleverTap validates if the coupon code is redeemable and returns the result in a structured response.

You can use this API in the following cases:

* When the user applies the coupon code on the cart.
* When a user enters the coupon code manually.
* When a user modifies the cart details after applying the coupon.
* Any other custom checkpoints in the user journey while completing a transaction.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/coupons/validate](https://in1.api.clevertap.com/1/promo/coupons/validate)

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

POST

# Query Parameters

| Parameter  | Description                                                                                                                             | Required/Optional                                                         | Type   | Sample Value |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------ | :----------- |
| `identity` | The unique identifier for the user. This is mandatory for campaign user coupon validation to determine the source of the coupon issued. | Optional for *All Users Coupon*  and Required for *Campaign Users Coupon* | String | `user123`    |

> 📘 Identity Not Passed
>
> When Identity is not passed for the campaign user coupon, the system returns an error message indicating the missing identity or prompts the user to log in to redeem the coupon code.

# Body Parameters

The body parameters define the structure and data required to validate a coupon. These parameters include key objects that provide information about the coupon, the associated order, items, and other contextual details necessary for the API to process the validation request.

## `coupon` Object

The `coupon` object contains `couponID` and `couponCode` for validating a coupon. Depending on the type of coupon, it includes either a coupon ID or coupon code, which is used to process and apply the coupon in various redemption scenarios.

| Parameter    | Description                                | Required/Optional | Data Type | Sample Value |
| ------------ | :----------------------------------------- | ----------------- | --------- | ------------ |
| `couponId`   | Coupon ID generated against a coupon code. | Optional          | String    | `coupon_123` |
| `couponCode` | Unique coupon code to be validated.        | Required          | String    | `DIWALI100`  |

> 📘 Coupon ID and Coupon Code Handling
>
> Coupon codes must be alphanumeric and are case-sensitive (must be entered in **ALL CAPS**). Ensure the codes follow the standard format defined during the setup process to avoid validation errors.
>
> The following are the conditions and their system handling for the `couponId` and `couponCode`:
>
> * **For Fixed Code Coupons**: For fixed codes, you can provide either a `couponCode` or a `couponID`. If both parameters are provided, the system validates the coupon using either one, as long as one of them is correct.
> * **For Bulk Code Coupons**: For bulk codes, you must provide the coupon code. The coupon ID and coupon code can have the same value, but the system only validates the coupon based on the provided `couponCode`. If the `couponCode` is invalid or not found, the system returns an error.

## `order` Object

The `order` object provides information about the overall order and is a mandatory parameter. If the `order` object is not included, the API cannot validate the coupon effectively, as the coupon applicability and calculations depend on these order details.

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
        Data Type
      </th>

      <th>
        Sample Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `orderId`
      </td>

      <td>
        Unique ID for the order being placed. This ID is used to track transactions and associate them with a specific purchase. For example, if a user is buying a shirt, the 

        `orderId`

         can be  

        `abcd12345`

        .
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        `abcd12345`
      </td>
    </tr>

    <tr>
      <td>
        `saleMrpTotal`
      </td>

      <td>
        Sum of the original price of all items, calculated as 

        `MRP`

         x 

        `quantity`
      </td>

      <td>
        Optional
      </td>

      <td>
        Numeric
      </td>

      <td>
        `500`
      </td>
    </tr>

    <tr>
      <td>
        `saleAmount`
      </td>

      <td>
        Sum of the selling price of all items, calculated as

        `unitPrice`

         x 

        `quantity`
      </td>

      <td>
        Required
      </td>

      <td>
        Numeric
      </td>

      <td>
        `300`
      </td>
    </tr>

    <tr>
      <td>
        `saleChannel`
      </td>

      <td>
        Source through which the sale is done.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `online`
      </td>
    </tr>

    <tr>
      <td>
        `location`
      </td>

      <td>
        Location of the sale made.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `India`
      </td>
    </tr>

    <tr>
      <td>
        `locationId`
      </td>

      <td>
        Unique ID assigned to a location.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `IND`
      </td>
    </tr>

    <tr>
      <td>
        `orderShipping`
      </td>

      <td>
        Sum of shipping charges at an order level.
      </td>

      <td>
        Optional
      </td>

      <td>
        Numeric
      </td>

      <td>
        `30`
      </td>
    </tr>

    <tr>
      <td>
        `orderStatus`
      </td>

      <td>
        <p>Indicates the current status of the transaction. This status is primarily managed within the user application and provides context for coupon redemption and transaction flow:</p><li>**Pending**: The order is initiated but not yet completed.</li><li>**Completed**: The order is successfully completed.</li><li>**Reverted**: The order is rolled back, and the coupon redemption is undone.</li>
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `pending`
      </td>
    </tr>

    <tr>
      <td>
        `paymentMode`
      </td>

      <td>
        Mode of payment used for the transaction. For example,

        `credit_card`

        , 

        `debit_card`

        , 

        `net_banking`

         and so on.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        `credit_card`
      </td>
    </tr>

    <tr>
      <td>
        `cartQuantity`
      </td>

      <td>
        Total quantity of items in the cart.
      </td>

      <td>
        Optional
      </td>

      <td>
        Numeric
      </td>

      <td>
        `4`
      </td>
    </tr>
  </tbody>
</Table>

## `item` Object

The `item` object provides details about individual items in the cart and is a mandatory parameter for coupon validation. If the item object is not included, the API cannot process the validation correctly, as it relies on item-specific information such as `productId`, `unitPrice`, and `quantity` to validate and apply coupons.

| Parameter     | Description                                                        | Required/Optional | Data Type | Sample Value    |
| ------------- | ------------------------------------------------------------------ | ----------------- | --------- | --------------- |
| `productId`   | Unique identifier for the product in the application.              | Required          | String    | `abc_124`       |
| `sku`         | Stock Keeping Unit (SKU) assigned to a product in the application. | Optional          | String    | `124`           |
| `itemName`    | Name of the product.                                               | Optional          | String    | `Coke`          |
| `category`    | Name of the product category.                                      | Optional          | String    | `shirts`        |
| `subCategory` | Name of the product subcategory.                                   | Optional          | String    | `cotton shirts` |
| `brand`       | Name of the product brand.                                         | Optional          | String    | `superdry`      |
| `unitPrice`   | Selling price of the product.                                      | Required          | Numeric   | `80`            |
| `mrp`         | MRP of the product.                                                | Optional          | Numeric   | `700`           |
| `quantity`    | Quantity of the product in the cart.                               | Required          | Numeric   | `12`            |

> 📘 `order` and `item` Object Validation
>
> The following 
>
> * If the `order` and `item` objects are not passed, the API returns an error message indicating missing order or item objects.
> * If the mandatory keys for the `order` and `item` objects are missing, the API returns an error message indicating the missing mandatory keys.
> * If the mandatory keys for the `order` and `item` objects are passed but contain empty values, the API returns an error message indicating the invalid input.
> * If the complete `order` and `item` object details are passed, the API validates the coupon based on the cart or product properties. It returns the `applicability` status, either `true` or `false`, along with a validation message.

## `metadata` Object

The `metadata` object allows you to include additional custom key-value pairs for advanced coupon validations.

| Parameter | Description                                                    | Required/Optional | Data Type | Sample Value |
| --------- | -------------------------------------------------------------- | ----------------- | --------- | ------------ |
| `key`     | Metadata property name (for example, color, size, supplier).   | Optional          | Object    | `size`       |
| `value`   | Metadata property value (for example, Red, Large, Supplier A). | Optional          | Object    | `large`      |

# Sample Request

Here is an example request to the Validate Coupon API, including headers needed to authenticate the request:

```curl
curl POST "https://in1.api.clevertap.com/1/promo/coupons/validate?identity=krishna123" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-data '{   
    "couponCode": "FLAT500",
    "order": {
        "orderId": "order12345",
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

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the *Settings* > *Project* page on the CleverTap dashboard.

<Image alt="Find CleverTap Project Details" align="center" border={true} src="https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png">
  Find CleverTap Project Details
</Image>

# Sample Response

```json
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
            "couponName": "FLAT500",
            "couponExpiry": 32494151843,
            "couponCodeId": 1003,
            "totalRedemptionUnlimited": true,
            "isApplicable": true,
            "description": "This offer is applicable on order above $100",
            "message": "",
            "couponExpiryNever": true,
            "termsAndConditions": [
                "Applicable on order above $100",
                "Valid till may end"
            ],
            "discountEffect": {
                "discountRate": 100,
                "rewardType": "discount",
                "discountType": "entireOrder",
                "discountAppliedOn": "saleAmount",
                "discountValue": "absolute"
            },
            "totalRedemptionLeft": 0,
            "couponSavings": {
                "customPropertyDiscount": null,
                "orderDiscount": 100,
                "shippingDiscount": 0,
                "orderCashbackPoints": 0,
                "order": {
                    "saleAmount": 101,
                    "metadata": {
                        "customerType": "guests"
                    },
                    "saleMrpTotal": 399,
                    "orderId": "order12345",
                    "orderStatus": "pending",
                    "itemsTotalQuantity": 0,
                    "itemsMrpTotal": 1000,
                    "saleChannel": "online",
                    "orderShipping": 0,
                    "locationId": "IND",
                    "location": "BLR",
                    "items": [
                        {
                            "unitPrice": 300,
                            "subCategory": "",
                            "metadata": null,
                            "quantity": 3,
                            "productId": "123456",
                            "itemDiscount": 90,
                            "mrp": 300,
                            "itemCashbackPoints": 0,
                            "grossAmount": 900,
                            "isItemValidForDiscount": true,
                            "itemName": "i1",
                            "grossMrp": 900,
                            "sku": "124",
                            "category": "electronics",
                            "itemNetAmount": 810,
                            "brand": "Zee5"
                        },
                        {
                            "unitPrice": 100,
                            "subCategory": "",
                            "metadata": null,
                            "quantity": 1,
                            "productId": "123456",
                            "itemDiscount": 10,
                            "mrp": 100,
                            "itemCashbackPoints": 0,
                            "grossAmount": 100,
                            "isItemValidForDiscount": true,
                            "itemName": "i1",
                            "grossMrp": 100,
                            "sku": "124",
                            "category": "electronics",
                            "itemNetAmount": 90,
                            "brand": "Zee5"
                        }
                    ],
                    "orderNetAmount": 1,
                    "itemsSaleAmount": 1000
                }
            },
            "bestCoupon": "",
            "identity": "j541399",
            "requestId": "",
            "redemptionActivationDate": 1755446400,
            "userRedemptionUnlimited": false,
            "redemptionStatus": true,
            "couponCode": "FLAT500",
            "userRedemptionLeft": 1
        }
    ]
}
```

# Understanding API Responses

The API response varies depending on the input payload, metadata, coupon configuration, and user identity. The following are the specific scenarios and their corresponding expected outcomes:

| Scenario                                | Behavior                                                                                                                                                                                                                                                                                                                                                              |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Optional parameters validation**      | If validation rules apply to optional parameters that are not provided in the request, the API still validates them and returns an error message.                                                                                                                                                                                                                     |
| **Metadata validation**                 | If you include custom fields in the API payload, these fields must be provided as key-value pairs within the metadata object of the `order` or `item` object. The API validates the metadata properties set in the coupon. It returns the applicability status, either `true` or `false` and a validation message, regardless of whether metadata details are passed. |
| **Coupon code or coupon ID validation** |                                                                                                                                                                                                                                                                                                                                                                       |
| Coupon details not provided             | The API returns an error message indicating that coupon details are mandatory.                                                                                                                                                                                                                                                                                        |
| Coupon expiration                       | The API returns an error message indicating the coupon is expired.                                                                                                                                                                                                                                                                                                    |
| Day-specific coupon availability        | If a coupon is not valid on certain days, the API behaves as defined in the [Fetch Coupon API](doc:fetch-coupon).                                                                                                                                                                                                                                                     |
| Case-sensitive coupon codes             | The API ignores case sensitivity for coupon code validation. For example, `KMN123` and `kmn123` are both considered valid codes.                                                                                                                                                                                                                                      |

# Errors

To know about the errors applicable to the Validate Coupon API, refer to [API Error Cases](doc:api-error-cases#coupon-api-errors).

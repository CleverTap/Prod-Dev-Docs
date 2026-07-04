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

The Validate Coupon API enables you to verify the validity and applicability of a coupon code entered by users at checkout. Verifying coupon details against cart and product metadata ensures accurate discounts and a seamless shopping experience.

You can integrate this API at relevant touchpoints within the application or website. Upon successful validation, CleverTap validates if the coupon code is redeemable and returns the result in a structured response.

You can use this API in the following cases:

- When the user applies the coupon code to the cart.
- When a user enters the coupon code manually.
- When a user modifies the cart details after applying the coupon.
- Any other custom checkpoints in the user journey while completing a transaction.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/promo/coupons/validate`

For region-specific endpoints, refer to [Region](doc:common-api-components#region).

## HTTP Method

POST

## Headers

For more information about API headers, refer to [Headers](doc:common-api-components#headers).

## Query Parameters

The following table lists the query parameters.

| Parameter | Description                                                                                                                     | Required/Optional                                                 | Type   | Example Value |
| :-------- | :------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------- | :----- | :------------ |
| identity  | The unique identifier for the user. Mandatory for campaign user coupon validation to determine the source of the coupon issued. | Optional for All Users Coupon, Required for Campaign Users Coupon | string | `user123`     |

<Callout icon="📘" theme="info">
  ### **Identity Not Passed**

  When identity is not passed for a campaign user coupon, the system returns an error message indicating the missing identity or prompts the user to log in to redeem the coupon code.
</Callout>

## Body Parameters

The body parameters define the structure and data required for coupon validation. The request body accepts a top-level `couponCode` field and an `order` object. The `items` array is nested inside the `order` object.

The following table lists the top-level body parameters.

| Parameter  | Description                                                                                                                              | Required/Optional | Type   | Example Value |
| :--------- | :--------------------------------------------------------------------------------------------------------------------------------------- | :---------------- | :----- | :------------ |
| couponCode | The unique coupon code to validate. Coupon code validation is case-insensitive, so `FLAT500` and `flat500` are treated as the same code. | Required          | string | `FLAT500`     |
| order      | Object containing order and item details. See the `order` Object section below.                                                          | Required          | object | —             |

### `order` Object

The `order` object provides information about the overall order. If the `order` object is not included, the API cannot validate the coupon effectively, as applicability and discount calculations depend on these order details.

The following table lists the `order` object parameters.

| Parameter     | Description                                                                                                                                                                    | Required/Optional | Type             | Example Value                |
| :------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------- | :--------------- | :--------------------------- |
| orderId       | Unique ID for the order being placed.                                                                                                                                          | Required          | string           | `abcd12345`                  |
| saleMrpTotal  | Sum of the original price of all items, calculated as MRP × quantity.                                                                                                          | Optional          | numeric          | `500`                        |
| saleAmount    | Sum of the selling price of all items, calculated as unitPrice × quantity.                                                                                                     | Required          | numeric          | `300`                        |
| saleChannel   | Source through which the sale is made.                                                                                                                                         | Optional          | string           | `online`                     |
| location      | Location where the sale is made.                                                                                                                                               | Optional          | string           | `India`                      |
| locationId    | Unique ID assigned to the location.                                                                                                                                            | Optional          | string           | `IND`                        |
| orderShipping | Shipping charges for the order.                                                                                                                                                | Optional          | numeric          | `30`                         |
| orderStatus   | Current status of the transaction. Accepted values: `pending` (order initiated but not completed), `completed` (order successfully completed), `reverted` (order rolled back). | Optional          | string           | `pending`                    |
| paymentMode   | Mode of payment used for the transaction (for example, `credit_card`, `debit_card`, `net_banking`).                                                                            | Optional          | string           | `credit_card`                |
| cartQuantity  | Total quantity of items in the cart.                                                                                                                                           | Optional          | numeric          | `4`                          |
| items         | Array of item objects nested inside `order`. See the `items` Array section below.                                                                                              | Required          | array of objects | —                            |
| metadata      | Custom key-value pairs for advanced coupon validations at the order level.                                                                                                     | Optional          | object           | `{"customerType": "guests"}` |

### `items` Array (nested inside `order`)

The `items` array is nested inside the `order` object and provides details about individual items in the cart. If the `items` array is not included, the API cannot process the validation correctly, as it relies on item-specific information such as `productId`, `unitPrice`, and `quantity` to validate and apply coupons.

The following table lists the `items` object parameters.

| Parameter   | Description                                                        | Required/Optional | Type    | Example Value       |
| :---------- | :----------------------------------------------------------------- | :---------------- | :------ | :------------------ |
| productId   | Unique identifier for the product in the application.              | Required          | string  | `abc_124`           |
| sku         | Stock Keeping Unit (SKU) assigned to the product.                  | Optional          | string  | `124`               |
| itemName    | Name of the product.                                               | Optional          | string  | `Cotton Shirt`      |
| category    | Category of the product.                                           | Optional          | string  | `shirts`            |
| subCategory | Subcategory of the product.                                        | Optional          | string  | `cotton shirts`     |
| brand       | Brand of the product.                                              | Optional          | string  | `superdry`          |
| unitPrice   | Selling price of the product.                                      | Required          | numeric | `80`                |
| mrp         | Original MRP of the product.                                       | Optional          | numeric | `700`               |
| quantity    | Quantity of the product in the cart.                               | Required          | numeric | `12`                |
| metadata    | Custom key-value pairs for advanced item-level coupon validations. | Optional          | object  | `{"color": "blue"}` |

<Callout icon="📘" theme="info">
  ### `order`**&#x20;and&#x20;**`items`**&#x20;Object Validation**

  The following scenarios apply when validating order and item objects:

  - If the `order` and `items` objects are not passed, the API returns an error message indicating missing order or item objects.
  - If the mandatory keys for the `order` and `items` objects are missing, the API returns an error message indicating the missing mandatory keys.
  - If the mandatory keys for the `order` and `items` objects are passed but contain empty values, the API returns an error message indicating the invalid input.
  - If the complete `order` and `items` object details are passed, the API validates the coupon based on the cart or product properties and returns the `isApplicable` status (`true` or `false`) along with a validation message.
</Callout>

### `metadata` Object

The `metadata` object allows you to include additional custom key-value pairs for advanced coupon validations. It can be passed within both the `order` and `items` objects.

The following table lists the `metadata` object parameters.

| Parameter | Description                                                    | Required/Optional | Type   | Example Value |
| :-------- | :------------------------------------------------------------- | :---------------- | :----- | :------------ |
| key       | Metadata property name (for example, color, size, supplier).   | Optional          | string | `size`        |
| value     | Metadata property value (for example, Red, Large, Supplier A). | Optional          | string | `large`       |

## Example Request

The following is a sample request to the Validate Coupon API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/promo/coupons/validate?identity=krishna123" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{
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
                "itemName": "Cotton Shirt",
                "category": "shirts",
                "subCategory": "cotton shirts",
                "brand": "superdry",
                "unitPrice": 300,
                "mrp": 300,
                "quantity": 3,
                "metadata": {
                    "color": "blue"
                }
            },
            {
                "productId": "123457",
                "sku": "125",
                "itemName": "Chino Trouser",
                "category": "trousers",
                "subCategory": "",
                "brand": "levis",
                "unitPrice": 100,
                "mrp": 100,
                "quantity": 1,
                "metadata": {
                    "color": "khaki"
                }
            }
        ],
        "metadata": {
            "customerType": "guests"
        }
    }
}'
```

### Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to **Settings** > **Project**.


<Image src="https://files.readme.io/739d78a63824aae1ab3162099ad19831000b8f090ce5960502d7fb7497def258-image.png" alt="Find CleverTap Project Details" align="center" border={true} />


## Example Response

The following is a sample response.

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
            "couponSavings": { "...": "..." },
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

The response structure is the same as the [Fetch Coupon API response](doc:fetch-coupon#response-fields). The following difference applies to the validate endpoint: the `bestCoupon` field returns an empty string (`""`) in the validate response, rather than `"Yes"` or `"No"`. This is because the best-coupon ranking is not computed during single-coupon validation.

## Understanding API Responses

The API response varies depending on the input payload, metadata, coupon configuration, and user identity. The following table describes the specific scenarios and their expected outcomes.

| Scenario                         | Behavior                                                                                                                                                                                                                                                                                                             |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Optional parameters validation   | If validation rules apply to optional parameters that are not provided in the request, the API still validates them and returns an error message.                                                                                                                                                                    |
| Metadata validation              | Custom fields must be provided as key-value pairs within the `metadata` object of the `order` or `items` object. The API validates the metadata properties set in the coupon and returns the `isApplicable` status (`true` or `false`) with a validation message, regardless of whether metadata details are passed. |
| Coupon details not provided      | The API returns an error message indicating that coupon details are mandatory.                                                                                                                                                                                                                                       |
| Coupon expired                   | The API returns an error message indicating the coupon is expired.                                                                                                                                                                                                                                                   |
| Day-specific coupon availability | If a coupon is not valid on the current day, the coupon is returned with `isApplicable` as `false` and an error message indicating the day restriction.                                                                                                                                                              |
| Case-sensitive coupon codes      | The API is case-insensitive when validating coupon codes. For example, `KMN123` and `kmn123`,both are considered valid codes.                                                                                                                                                                                        |

## Errors

To know about the errors applicable to the Validate Coupon API, refer to [API Error Cases](doc:api-error-cases#coupon-api-errors).

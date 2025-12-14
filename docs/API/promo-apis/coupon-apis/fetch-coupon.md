---
title: Fetch Coupon
excerpt: >-
  Learn how to use the Fetch Coupon API to retrieve and display applicable
  coupons, enhancing user experience dynamically.
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

The Fetch Coupon API enables you to create a coupon tray within your application user interface by consuming the coupon details returned in the API response. It boosts purchase conversion by showing applicable coupons to users based on product listing metadata or cart details.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/coupons](https://in1.api.clevertap.com/1/promo/coupons)

## Region

To identify the API endpoint for the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

POST

# Query Parameters

The query parameters are optional, and coupons are fetched based on metadata (for example, category, brand, and so on).

| Parameter        | Description                                                                                                                                                                                                    | Required/Optional                    | Data Type | Sample Value       |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | --------- | ------------------ |
| `identity`       | Unique identifier for the user.                                                                                                                                                                                | Optional                             | String    | `user123`          |
| `metadata key`   | Key representing the product metadata property used to filter coupons. Supports multiple Key-Value pairs in a comma-separated format.                                                                          | Optional                             | String    | `["key1", "key2"]` |
| `metadata value` | Value associated with the metadata key to fetch applicable coupons. For example, if the metadata key is `category`, and the coupon applies to the category `Jeans`, the expected value must be set to `Jeans`. | Optional                             | String    | `Jeans`            |
| `condition`      | Coupon filter condition based on metadata key-value. Default value is `OR`.                                                                                                                                    | Optional if the value is not passed. | ENUM      | `OR/AND`           |

In the query parameters, you can pass multiple values as comma-separated entries. The system interprets the query parameters as follows:

* **OR Condition**: If multiple values are passed for a single key, the system treats it as an OR condition. For example, `metadata_key=category&expected_value=Jeans,Shirts`. This returns coupons applicable for the `category` as either `Jeans` or `Shirts`.
* **AND Condition**: If multiple keys are passed, the system treats it as an AND condition. For example, `metadata_key=category=Shirt;brand=Levis`. This returns coupons applicable for the `category` as `Jeans` *and* the `brand` as `Nike`.

# Body Parameters

The Fetch Coupon API request includes the following objects: `order` and `items`.  The API returns detailed information about each coupon's properties, applicability, and validation.

## `order` Object

The `order` object provides information about the overall order. This is optional when fetching coupons but is required for validating coupon applicability based on order details, such as pricing, shipping, and payment mode.

| Parameter       | Description                                                                   | Required/Optional | Data Type | Sample Value |
| --------------- | ----------------------------------------------------------------------------- | ----------------- | --------- | :----------- |
| `orderId`       | Unique identifier for the order.                                              | Optional          | String    | `ORD123456`  |
| `saleMrpTotal`  | Sum of the original price (MRP) of all items, that is, `MRP` x `quantity`.    | Optional          | Numeric   | `1500.00`    |
| `saleAmount`    | Sum of the selling price of all the items, that is, `unitPrice` x `quantity`. | Optional          | Numeric   | `1200.00`    |
| `saleChannel`   | Source of the sale (for example, online, offline)                             | Optional          | String    | `online`     |
| `location`      | Location where the sale is made.                                              | Optional          | String    | `India`      |
| `locationId`    | Unique identifier for location.                                               | Optional          | String    | `LOC98765`   |
| `orderShipping` | Shipping charges for the order.                                               | Optional          | Numeric   | `50.00`      |
| `orderStatus`   | Status of the order, that is, pending, completed, or reverted.                | Optional          | String    | `completed`  |
| `paymentMode`   | Payment mode of the transaction (for example, cash, UPI, card)                | Optional          | String    | `UPI`        |
| `cartQuantity`  | Total product quantity added to the cart.                                     | Optional          | Numeric   | `4`          |

## `items` Object

The `items` object provides the following details about individual items present in the cart:

| Parameter     | Description                                                        | Required/Optional | Data Type | Sample Value       |
| ------------- | ------------------------------------------------------------------ | ----------------- | --------- | :----------------- |
| `productId`   | Unique identifier for the product present in your product catalog. | Optional          | String    | `PROD12345`        |
| `sku`         | Stock Keeping Unit (SKU) of the product.                           | Optional          | String    | `SKU98765`         |
| `itemName`    | Name of the product.                                               | Optional          | String    | `wireless earbuds` |
| `category`    | Category of the product.                                           | Optional          | String    | `electronics`      |
| `subCategory` | Subcategory of the product.                                        | Optional          | String    | `light`            |
| `brand`       | Brand of the product (for example, Nike)                           | Optional          | String    | `Coca-Cola`        |
| `mrp`         | Original price (MRP).                                              | Optional          | Numeric   | `2000.00`          |
| `unitPrice`   | Selling price of the product.                                      | Optional          | Numeric   | `1500.00`          |
| `quantity`    | Quantity of the product.                                           | Optional          | Numeric   | `2`                |

## `metadata` Object

The `metadata` object allows you to include additional custom key-value pairs for advanced coupon validations. This provides flexibility in defining coupon applicability based on specific business requirements. The object can be passed within both the `order` and `items` objects.

| Parameter | Description                                                    | Required/Optional | Data Type | Sample Value |
| --------- | -------------------------------------------------------------- | ----------------- | --------- | :----------- |
| `key`     | Metadata property name (for example, color, size, supplier).   | Optional          | String    | `color`      |
| `value`   | Metadata property value (for example, Red, Large, Supplier A). | Optional          | String    | `red`        |

The `metadata` key-value pairs enhance the coupon validation logic by allowing custom attributes to be considered during the validation process. For example, a coupon could be applicable only for items of a specific color (Red) or size (Large) or a specific supplier (Supplier A). By including such metadata, the system validates the coupon based on these additional criteria, ensuring accurate coupon applicability.

> 📘 Key Points to Remember for `metadata` Object
>
> * CleverTap recommends passing the `order` and `items` object details when calling the Fetch Coupon API on the checkout page. This displays the right coupons to the users in the coupon tray.
> * The following are the specific scenarios and their corresponding expected outcomes when validating `metadata` object for the API response:
>   * If the `metadata` object is not passed, the API returns `applicability` as `true`, bypassing all validations.
>   * If the `metadata` object is passed, the API validates metadata key properties set in the coupon and returns `applicability` as `true`/`false` along with validation messages.

# Sample Request

Here is an example request to the Fetch Coupon API, including headers needed to authenticate the request:

```curl
curl POST "https://in1.api.clevertap.com/1/promo/coupons?identity=BDF0V2" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
--data '{   
    "couponCode": "CODE_1749462472681",
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
            "category": "shirts",
            "subCategory": "",
            "brand": "levis",
            "unitPrice":300,
            "mrp":300,
            "quantity": 3,
            "metadata": {
                "color": "blue"
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
                "color": "blue"
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
            "couponName": "",
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
                "orderCashbackPoints": 99,
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
                            "itemDiscount": 0,
                            "mrp": 300,
                            "itemCashbackPoints": 89.1,
                            "grossAmount": 900,
                            "isItemValidForDiscount": true,
                            "itemName": "i1",
                            "grossMrp": 900,
                            "sku": "124",
                            "category": "shirts",
                            "itemNetAmount": 810.9,
                            "brand": "levis"
                        },
                        {
                            "unitPrice": 100,
                            "subCategory": "",
                            "metadata": null,
                            "quantity": 1,
                            "productId": "123456",
                            "itemDiscount": 0,
                            "mrp": 100,
                            "itemCashbackPoints": 9.9,
                            "grossAmount": 100,
                            "isItemValidForDiscount": true,
                            "itemName": "i1",
                            "grossMrp": 100,
                            "sku": "124",
                            "category": "electronics",
                            "itemNetAmount": 90.1,
                            "brand": "Zee5"
                        }
                    ],
                    "orderNetAmount": 2,
                    "itemsSaleAmount": 1000
                }
            },
            "bestCoupon": "Yes",
            "identity": "j6158301",
            "requestId": "",
            "redemptionActivationDate": 1756405800,
            "userRedemptionUnlimited": false,
            "redemptionStatus": true,
            "couponCode": "SALE100",
            "userRedemptionLeft": 1
        },
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
            "couponName": "",
            "couponExpiry": 1756489069,
            "couponCodeId": 2167,
            "totalRedemptionUnlimited": true,
            "isApplicable": true,
            "description": "Offer applicable on shirts category",
            "message": "",
            "couponExpiryNever": false,
            "termsAndConditions": [
                "Valid till month end"
            ],
            "discountEffect": {
                "discountRate": 10,
                "rewardType": "discount",
                "discountType": "specificProducts",
                "discountAppliedOn": "grossAmount",
                "discountValue": "percentage"
            },
            "totalRedemptionLeft": 0,
            "couponSavings": {
                "customPropertyDiscount": null,
                "orderDiscount": 90,
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
                    "itemsTotalQuantity": 3,
                    "itemsMrpTotal": 900,
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
                            "itemDiscount": 0,
                            "mrp": 300,
                            "itemCashbackPoints": 89.1,
                            "grossAmount": 900,
                            "isItemValidForDiscount": true,
                            "itemName": "i1",
                            "grossMrp": 900,
                            "sku": "124",
                            "category": "shirts",
                            "itemNetAmount": 810.9,
                            "brand": "levis"
                        },
                        {
                            "unitPrice": 100,
                            "subCategory": "",
                            "metadata": null,
                            "quantity": 1,
                            "productId": "123456",
                            "itemDiscount": 0,
                            "mrp": 100,
                            "itemCashbackPoints": 9.9,
                            "grossAmount": 100,
                            "isItemValidForDiscount": true,
                            "itemName": "i1",
                            "grossMrp": 100,
                            "sku": "124",
                            "category": "electronics",
                            "itemNetAmount": 90.1,
                            "brand": "Zee5"
                        }
                    ],
                    "orderNetAmount": 910,
                    "itemsSaleAmount": 900
                }
            },
            "bestCoupon": "No",
            "identity": "j6158301",
            "requestId": "",
            "redemptionActivationDate": 1755877500,
            "userRedemptionUnlimited": false,
            "redemptionStatus": true,
            "couponCode": "SHIRTS10",
            "userRedemptionLeft": 1
        }
    ]
}
```

On a successful API call, the API returns detailed information about each coupon's properties, applicability, and validation.

| Key                        | Definition                                                                                                                                                                                                                                                                                                             | Type            | Sample Value                        |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ----------------------------------- |
| `identity`                 | Unique customer identifier.                                                                                                                                                                                                                                                                                            | String          | `user123`                           |
| `couponCode`               | Fixed coupon code configured in CleverTap.                                                                                                                                                                                                                                                                             | String          | `DISCOUNT20`                        |
| `couponCodeId`             | Unique coupon identifier.                                                                                                                                                                                                                                                                                              | String          | `12345`                             |
| `couponName`               | Name of the coupon.                                                                                                                                                                                                                                                                                                    | String          | `20% Off on Electronics`            |
| `description`              | Description of the coupon.                                                                                                                                                                                                                                                                                             | String          | `Get 20% off on all electronics.`   |
| `termsAndConditions`       | Array of terms and conditions associated with the coupon.                                                                                                                                                                                                                                                              | String          | `[ "Valid for electronics only." ]` |
| `totalRedemptionLeft`      | Total redemptions available; `unlimited` if no limit is set.                                                                                                                                                                                                                                                           | String          | `unlimited`                         |
| `userRedemptionLeft`       | Remaining redemptions for the user; `unlimited` if the total redemption limit is unlimited.                                                                                                                                                                                                                            | Numeric         | `5`                                 |
| `isApplicable`             | Indicates if the coupon is applicable.                                                                                                                                                                                                                                                                                 | Boolean         | `true`                              |
| `redemptionStatus`         | Indicates if the coupon is redeemable.                                                                                                                                                                                                                                                                                 | Boolean         | `false`                             |
| `redemptionActivationDate` | Activation date for coupon redemption.                                                                                                                                                                                                                                                                                 | Timestamp       | `2024-12-01T00:00:00Z`              |
| `message`                  | Validation message if `isApplicable` is `false`.                                                                                                                                                                                                                                                                       | String          | `Coupon expired.`                   |
| `bestCoupon`               | Identifies the coupon offering the highest discount. This is determined by comparing discount amounts for all applicable coupons where `key` = `orderDiscount`. For example, if there are two coupons, one offering a discount of $10 and the other $15, the `bestCoupon` will refer to the one with the $15 discount. | String          | `No`                                |
| `couponSavings`            | Array containing derived order properties related to coupon application, such as total savings and item-specific discounts.                                                                                                                                                                                            | Array of object |                                     |
| `totalRedemptionUnlimited` | Indicates the total redemption limit per code is set to unlimited.                                                                                                                                                                                                                                                     | Boolean         | `true`                              |
| `userRedemptionUnlimited`  | Indicates the total redemption limit per user is set to unlimited.                                                                                                                                                                                                                                                     | Boolean         | `false`                             |
| `redemptionStatus`         | Indicates whether the coupon redemption is active for the user or not.                                                                                                                                                                                                                                                 | Boolean         | `true`                              |

## `order` Object Keys

| Key                      | Definition                                                                                                                                                   | Type    | Sample Value                                    |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | ----------------------------------------------- |
| `orderNetAmount`         | Net order amount after applying the discount, that is, `saleAmount` - `orderDiscount`. (Negative values are returned as 0).                                  | Numeric | `500`                                           |
| `orderDiscount`          | Total discount applied to the order.                                                                                                                         | Numeric | `100`                                           |
| `shippingDiscount`       | Discount applied to shipping charges.                                                                                                                        | Numeric | `50`                                            |
| `customPropertyDiscount` | Discount applied to a custom property. If applicable, the API returns an object containing the property name and discount value; otherwise, it returns null. | Object  | `{ "property": "orderShipping", "value": 100 }` |
| `property`               | Name of the custom property on which the discount is applied.                                                                                                | String  | `orderShipping`                                 |
| `value`                  | Discount amount applied to the custom property.                                                                                                              | Numeric | `100`                                           |
| `orderCashbackPoints`    | Total cashback points for the order.                                                                                                                         | Numeric | `20`                                            |
| `itemsSaleAmount`        | Total sale amount of the orders being ordered. Calculated as `unitPrice` \* Quantity of all the applicable items for the discount.                           | Numeric | `500`                                           |
| `itemsMrpTotal`          | Total MRP amount of the orders being ordered. Calculated as `MRP` \* Quantity of all the applicable items for the discount.                                  | Numeric | `600`                                           |

## `items` Object Keys

| Key                  | Definition                                                                                                      | Type    | Sample Value |
| -------------------- | --------------------------------------------------------------------------------------------------------------- | ------- | ------------ |
| `itemDiscount`       | Discount applied at the item level which is proportionate to the order discount.                                | Numeric | `10`         |
| `itemNetAmount`      | Net amount for an item after applying the discount, that is, `(unitPrice x  quantity) - itemDiscount`.          | Numeric | `90`         |
| `itemCashbackPoints` | Cashback points applied to the item.                                                                            | Numeric | `5`          |
| `grossAmount`        | Gross amount of the total items added being ordered. Calculated as `unitPrice` \* Quantity of respective items. | Numeric | `300`        |
| `grossMRP`           | Gross MRP of the total items added being ordered. Calculated as `MRP` \* Quantity of respective items.          | Numeric | `400`        |

> 📘 `order` and `item` Object Validation
>
> The following are the specific scenarios and their corresponding expected outcomes when validating `order` and `item` objects for the API response:
>
> * If the `order` and `item` objects are not provided, the API returns `applicability` as `true`, bypassing all validations.
> * If the `order` and `item` objects exist, but all keys have empty values, the API returns `applicability` as `true`, bypassing all validations.
> * If the `order` and `item` objects are passed with key values, the API validates coupons based on discount rules referencing order and item details and returns coupons with their `applicability` status.

## `coupon` Object Keys

<Table>
  <thead>
    <tr>
      <th>
        Key
      </th>

      <th>
        Definition
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
        `discountEffect`
      </td>

      <td>
        Details of the discount applied.
      </td>

      <td>
        Object
      </td>

      <td>
        `{ "discountValue": "percentage", "discountRate": 50, "discountType": "entireOrder", "discountAppliedOn": "saleAmount" }`
      </td>
    </tr>

    <tr>
      <td>
        `discountValue`
      </td>

      <td>
        Type of discount applied, for example, percentage or absolute.
      </td>

      <td>
        String
      </td>

      <td>
        ```
        percentage,
         absolute
        ```
      </td>
    </tr>

    <tr>
      <td>
        `discountRate`
      </td>

      <td>
        Discount percentage, for example, 50 for a flat 50% discount.
      </td>

      <td>
        Numeric
      </td>

      <td>
        `50`
      </td>
    </tr>

    <tr>
      <td>
        `discountType`
      </td>

      <td>
        Type of coupon determining how the discount is applied. The following are the possible values:<li>`entireOrder`: Applies the discount to the total cart value.</li><li>`cartExcluding`: Excludes specific items from the discount calculation.</li><li>`specificProducts`: Applies the discount to specific items only.</li><li>`customProperty`: Discounts are determined by custom-defined properties such as cart metadata.</li>
      </td>

      <td>
        String
      </td>

      <td>
        `entireOrder`
      </td>
    </tr>

    <tr>
      <td>
        `discountAppliedOn`
      </td>

      <td>
        Property on which the discount is applied, for example, `saleAmount`, `itemsSaleAmount`, `orderShipping`, or a custom property key.
      </td>

      <td>
        String
      </td>

      <td>
        `saleAmount`
      </td>
    </tr>
  </tbody>
</Table>

## `couponTimeframe` Object

The `couponTimeframe` object defines the coupon redemption time slots for either all days or specific custom-selected days.

* If a *Custom schedule* is not set, the coupon is valid for 24 hours on all days.
* If a *Custom schedule* is set, the API returns specific days and their corresponding time slots.

| Key               | Description                                                                                                                                                                                                                                                                                                                     | Type            | Sample Value      |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ----------------- |
| **validationDay** | The days when the coupon is valid. If the *Custom schedule* is not set, the API returns `All`, meaning the coupon is valid every day. If the *Custom schedule* is set, the API returns specific day names. For example, `MONDAY`, `TUESDAY`).                                                                                   | String          | `ALL` or `MONDAY` |
| **timeSlot**      | Defines the time slots when the coupon can be used. If the *Custom schedule* is set, this array includes `startTime`, and `endTime` for each selected day. If the *Custom schedule* is not set, it includes a single `startTime`, `endTime`, and `validationDay = All` (indicating the coupon is valid 24 hours on all 7 days). | Object          |                   |
| **startTime**     | The start time of the valid time slot for each day in `HH:MM:SS` format. If the *Custom schedule* is set, it contains different values per day. If the *Custom schedule* is not set, the values default to `00:00:00 AM`.                                                                                                       | Timestamp       | `06:00:00 AMPM`   |
| **endTime**       | The end time of the valid time slot for each day in `HH:MM:SS` format. If the *Custom schedule* is set, it contains different values per day. If the *Custom schedule* is not set, the values default to `23:59:59 PM`.                                                                                                         | Timestamp       | `20:00:00 `       |
| **couponExpiry**  | The overall expiration timestamp after which the coupon can no longer be used.                                                                                                                                                                                                                                                  | Epoch Timestamp | `1740829235`      |

### Sample Value When Custom Schedule is Not Set (Valid 24/7)

```json
"couponTimeframe": {
    "timeSlot": {
        "startTime": "00:00:00",
        "endTime": "23:59:59"
    },
    "validationDay": "All"
}
```

### Sample Value When Custom Schedule is Set (Specific Days and Time Slots)

```json
"couponTimeframe": [
    {
        "validationDay": "MONDAY",
        "timeSlot": {
            "startTime": "18:00:00",
            "endTime": "20:00:00"
        }
    },
    {
        "validationDay": "TUESDAY",
        "timeSlot": {
            "startTime": "18:00:00",
            "endTime": "20:00:00"
        }
    }
]
```

# Understanding API Responses

The API response varies depending on the input payload, metadata, coupon configuration, and user identity. The following are the specific scenarios and their corresponding expected outcomes:

| Scenario                                                                        | Behavior                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Empty Payload**                                                               | Returns all the `All Users` coupons with `applicability` as `true`, bypassing any validations set on the coupon.                                                                                                                                                                                                                                                                                                                                     |
| **Identity Passed (Logged in user)**                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Without order or item details                                                   | Returns `All Users` and campaign coupons applicable for the identity, bypassing validations.                                                                                                                                                                                                                                                                                                                                                         |
| With order or item details                                                      | Returns `All Users` and campaign coupons for the identity, with validations checked. Applicability is `true` or`false` with applicable error messages.                                                                                                                                                                                                                                                                                               |
| **Identity Not Passed (Non logged in user)**                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Without order and item objects                                                  | Returns only `All Users` coupons with `applicability` as `true`, excluding those with validations set on user properties.                                                                                                                                                                                                                                                                                                                            |
| With order and item objects                                                     | Returns only `All Users` coupons with validations checked. Applicability is `true`/`false` with applicable error messages.                                                                                                                                                                                                                                                                                                                           |
| **Case-Sensitive Values**                                                       | Case-sensitive values are ignored when validating coupons on product or cart properties passed in the API payload with coupon configuration values. For example, a product name `APPLE` is set in the coupon, but the value `apple` is passed in the API payload. `APPLE` matches `apple` in this case.                                                                                                                                              |
| **Code Visibility Unchecked in Configuration**                                  | Coupons with unchecked code visibility are not returned in the API response. The user is expected to enter the coupon code manually.                                                                                                                                                                                                                                                                                                                 |
| **Coupon Availability on Specific Days**                                        | If the coupon is unavailable on specific days, it is returned with `applicability` as `false` and an error message.                                                                                                                                                                                                                                                                                                                                  |
| **Coupon Status**                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Active                                                                          | `All Users` coupons are returned with `applicability` as `true`/`false`. Rule-based coupons are returned with both `applicability` and `redemption status`.                                                                                                                                                                                                                                                                                          |
| Scheduled, Expired, Draft, Ended, Archived                                      | Coupons are not returned.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Rule-Based Coupons with Multiple Records**                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Same coupon assigned multiple times to the same user                            | Returns each coupon as an independent entry along with the following properties: activation and expiry dates.                                                                                                                                                                                                                                                                                                                                        |
| Multiple records of the same coupon exist                                       | Returns each record independently if the configuration status is `active` and redemption status is `active` or`inactive`.                                                                                                                                                                                                                                                                                                                            |
| **Redemption Limits**                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Rule-based coupon**: Per-user redemption limit exceeded for rule-based coupon | Returns the coupon with the`applicability` as `false`.                                                                                                                                                                                                                                                                                                                                                                                               |
| Per-code redemption limit exceeded for rule-based coupon                        | Returns the specific coupon code with the`applicability` as `false`.                                                                                                                                                                                                                                                                                                                                                                                 |
| Per-user or per-code redemption limit exceeded for all users coupon             | Returns the coupon with the`applicability` as `false`.                                                                                                                                                                                                                                                                                                                                                                                               |
| **Coupon Sorting**                                                              | Adds a key `bestCoupon` with a value of `Yes`/`No`, indicating the highest discount amount based on the `orderDiscount` key.                                                                                                                                                                                                                                                                                                                         |
| **Metadata Key-Value Matching in Query Parameters**                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Specific Products coupon                                                        | Matches the query Key-Value with product properties configured in the coupon. For example, `category` = `jeans`, `shirts`.                                                                                                                                                                                                                                                                                                                           |
| Entire Order or Custom Property coupon                                          | Returns all `entire order` and `custom property` coupons, as they do not have product properties.                                                                                                                                                                                                                                                                                                                                                    |
| Entire Order coupon with exclude products condition                             | Returns all `entire order` coupons not matching with query Key-Value.                                                                                                                                                                                                                                                                                                                                                                                |
| **Fetch Coupon by User Properties**                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Coupon has validations set on user properties                                   | Returns the coupon only when identity is passed. It does not include these coupons when identity is not passed.                                                                                                                                                                                                                                                                                                                                      |
| Validation                                                                      | You do not need to pass user property values in the API payload. The system automatically evaluates the user properties associated with the given identity against the coupon conditions. For example, if a coupon is available to users whose gender is `male` and the country is `India`, the system checks these properties for the provided identity. If the identity matches these conditions, the coupon will be included in the API response. |

# Errors

To know about the errors applicable to the Fetch Coupon API, refer to [API Error Cases](doc:api-error-cases#coupon-api-errors).

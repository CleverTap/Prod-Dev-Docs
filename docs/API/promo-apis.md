---
title: Promo APIs
excerpt: >-
  Discover how to create impactful loyalty and promotional campaigns with
  CleverTap Promo APIs.
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

The Promo APIs suite provides the capability of wallets and coupons, offering marketers a comprehensive toolkit to enhance customer engagement and drive loyalty. These APIs empower businesses to create seamless loyalty programs and manage discount campaigns, enabling customers to earn, redeem, and enjoy rewards effortlessly.

The Promo APIs suite has two categories:

* [Wallet APIs](doc:wallet-apis)
* [Coupon APIs](doc:coupon-apis)
* [Partner Vouchers API](doc:partner-vouchers-api) 

You can find more information about each Promo API here to help implement efficient promotional capabilities. 

## Wallet APIs

| API Name                                               | Scope                                                                                             | Example/Use Case                                                |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| [Get Wallet Details](doc:get-wallet-details)           | Fetches wallet details, including current balance and wallet metadata for a user.                 | A user views their wallet balance on the app dashboard.         |
| [Get Wallet Transactions](doc:get-wallet-transactions) | Retrieves the transaction history for a wallet, including debits and credits.                     | A user checks their past transactions to match their spending.  |
| [Debit/Credit](doc:debitcredit-wallet)                 | Allows you to add or deduct reward points from a user's wallet based on their activity or events. | Rewarding a user with loyalty points for completing a purchase. |

## Coupon APIs

| API Name                                        | Scope                                                                                                    | Example/Use Case                                                                       |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| [Fetch Coupon](doc:fetch-coupon)                | Retrieves all available coupons for a user, including general and user-specific coupons.                 | A user views a list of active coupons before making a purchase.                        |
| [Validate Coupon](doc:validate-coupon)          | Validates a coupon against business rules such as expiry date, user eligibility, or transaction details. | To check if a coupon code is valid before applying it during checkout.                 |
| [Redeem/Revert Coupon](doc:redeemrevert-coupon) | Redeems or reverts a coupon for a user based on its usage status.                                        | Redeeming a discount coupon for a purchase or reverting it after a failed transaction. |

## Partner Voucher API

| API Name                           | Scope                                                                 | Example/Use Case                                                 |
| :--------------------------------- | :-------------------------------------------------------------------- | :--------------------------------------------------------------- |
| [Fetch Voucher](doc:fetch-voucher) | Retrieves all available partner voucher codes that users have earned. | A user views a list of active vouchers before making a purchase. |

# General Guidelines for Wallet and Coupon APIs

* **Authentication**: All Promo API requests must be authenticated using the Basic Authentication method with the following keys:
  * **User**: API Key
  * **Password**: Secret Key
* **Primary Identifier**: The primary identifier varies based on the type of API. 
  * **For Wallet APIs**:
    * The [Wallet Details](doc:get-wallet-details) API uses `identity` as its primary identifier for wallet integration.
    * The [Wallet Transactions](doc:get-wallet-transactions) and [Debit/Credit Wallet](doc:debitcredit-wallet) API utilize both `identity` and `walletId` as their primary identifiers for wallet integration.
  * **For Coupon APIs**:
    * In [Fetch Coupon](doc:fetch-coupon) if the `identity` parameter is provided, the API also returns user-specific coupons linked to the provided `identity` along with the general coupons. Without `identity`, only general coupons are retrieved.
    * The [Validate Coupon](doc:validate-coupon) and [Redeem/Revert Coupon](doc:redeemrevert-coupon) APIs utilize both `identity` and `couponCode` as their primary identifiers for coupon-related operations.
* **Response Format**: JSON
* **Error Codes**:
  * The standard error cases (e.g., 200 Success, 400 Client Error, or 500 Server Error) align with the existing CleverTap API behavior. To read more about the system API errors applicable across all CleverTap API requests, refer to [Generic API Codes](docs:api-errors#generic-api-codes). 
  * For custom error codes for specific issues, such as invalid point values, expired coupons, or redemption limits, refer to the following: 
    * [Wallet API Errors](doc:api-error-cases#wallet-api-errors)
    * [Coupon API Errors](doc:api-error-cases#coupon-api-errors)
    * [Partner Voucher API Errors](doc:api-error-cases#partner-voucher-api-errors)

# Glossary Terms

The following glossary defines the key terms and labels used in the Promo APIs, covering coupon creation, validation, and configuration.

### All Users Coupon

A coupon type available to all users without requiring any qualification or predefined rules.

### Rule-based Coupon

A coupon that applies only to users who meet specific qualifying rules or conditions, such as user actions or properties.

### Single Code Coupon

A coupon with the same code shared among all users. It can be used by anyone, with or without meeting specific conditions, depending on the configuration.

### Bulk Codes (Downloadable)

A series of unique coupon codes that can be downloaded and used once per user. These codes are not tied to any specific user actions or qualifications.

### Bulk Codes (Rule-based)

A series of unique coupon codes that apply only to users who meet specific conditions. Each code can be used once per qualifying user.

### Entire Cart

A reward effect where the coupon is applied to all items in the cart, providing a discount across the entire order.

### Specific Products

A reward effect where the coupon is applied only to specific items in the cart as defined during coupon creation.

### Entire Cart Exclude Products

A reward effect where the coupon is applied to all items in the cart except those explicitly excluded.

### Custom Property Coupon

A coupon configured to apply to custom order properties, such as shipping fees or other specific order-level attributes.

### Shipping Discount

A reward effect where the coupon specifically provides a discount on shipping charges.

### User Property-based Coupon

Coupon that applies to users based on predefined user attributes, such as location, purchase history, or loyalty tier.

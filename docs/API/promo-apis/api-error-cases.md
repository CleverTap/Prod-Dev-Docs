---
title: API Error Cases
excerpt: >-
  Understand the custom error responses you may encounter when making requests
  to the Wallet and Coupon APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Wallet API Errors

The following are the custom API errors that you may encounter upon failed requests of the Wallet APIs:

| Error Code | Error Case                                                 | Error Details                                                                                                            | Applicable API              |
| ---------- | :--------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | :-------------------------- |
| 777        | Invalid or incorrect `wallet_Id`                           | The specified Wallet ID is invalid or does not exist.                                                                    | All Wallet APIs             |
| 400        | Missing required path parameter                            | The path parameter key is missing. For example, calling a wallet API without `walletId` in the path.                     | All Wallet APIs             |
| 777        | Invalid date format, expected in epoch                     | `from` or `to` was passed as a formatted date string. For example, `2026-01-01` instead of a Unix epoch timestamp.       | Get Wallet Transactions API |
| 777        | Invalid date format                                        | `from` or `to` was passed with a non-numeric or malformed epoch value.                                                   | Get Wallet Transactions API |
| 777        | Invalid date range                                         | The `from` value is greater than the `to` value.                                                                         | Get Wallet Transactions API |
| 777        | Multiple values are not supported against a query param    | More than one value was passed for a filter key that only accepts a single value. For example, `txnSource=api,campaign`. | Get Wallet Transactions API |
| 777        | Invalid query parameter                                    | An unrecognized query parameter key was passed. For example, `sale_channel` instead of `saleChannel`.                    | Get Wallet Transactions API |
| 777        | Invalid value for `type`                                   | An invalid value was passed for the type parameter. For example, `type=credited`.                                        | Get Wallet Transactions API |
| 777        | Invalid `transactionType`                                  | This error occurs when the `transactionType` key is invalid. It must be either `DEBIT` or `CREDIT`.                      | Debit/Credit Wallet API     |
| 777        | Missing mandatory key in the payload                       | `KEY_NAME` is required.                                                                                                  | Debit/Credit Wallet API     |
| 500        | Incorrect data type for `points`                           | The `points` field must be numeric.                                                                                      | Debit/Credit Wallet API     |
| 777        | Negative points passed in the request                      | `points` must always be a positive value.                                                                                | Debit/Credit Wallet API     |
| 777        | Debit points exceed the active points in the user's wallet | The points to be debited cannot exceed the user's available wallet balance.                                              | Debit/Credit Wallet API     |
| 777        | Metadata is provided, but is an empty object (\{})         | The metadata key value can not be empty                                                                                  | Debit/Credit Wallet API     |

# Coupon API Errors

The following are the custom API errors that you may receive upon failed requests of the Coupon APIs:

| Error Codes | Error Case                                                                             | Error Details                                                                              | Applicable API           |
| ----------- | :------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | :----------------------- |
| 777         | Wrong field added in request body or mandatory fields not passed                       | A required field `KEY_NAME` is missing in the request body.                                | All APIs                 |
| 777         | Coupon is expired                                                                      | The coupon code has expired and is no longer valid.                                        | All APIs                 |
| 777         | Total coupon redemption limit exceeded                                                 | The redemption limit for the coupon has been exceeded.                                     | All APIs                 |
| 777         | Per-user redemption limit exceeded                                                     | The per-user redemption limit for the coupon has been exceeded.                            | All APIs                 |
| 777         | Daily/Weekly/Monthly redemption exceeded                                               | The coupon's daily, weekly, or monthly redemption limit has been exceeded.                 | All APIs                 |
| 777         | Coupon not available for specific days or times                                        | The coupon is not valid today or at this time.                                             | All APIs                 |
| 777         | Coupon cart validation failed                                                          | The coupon cart validation failed based on the specified conditions.                       | All APIs                 |
| 777         | Coupon product validation failed                                                       | Product validation failed due to invalid conditions.                                       | All APIs                 |
| 777         | User property validation failed                                                        | User property validation failed due to the specified conditions.                           | All APIs                 |
| 777         | Selected items: No product property value matched                                      | The selected item’s properties do not match the coupon requirements.                       | All APIs                 |
| 777         | Applied coupon code is expired                                                         | The applied coupon code has expired.                                                       | Validate and Redeem APIs |
| 777         | Coupon code or ID missing in the request body                                          | The coupon code or ID is missing or invalid in the request body.                           | Validate and Redeem APIs |
| 777         | Non-redeemed coupon is reverted                                                        | The coupon cannot be reverted as it was never redeemed.                                    | Validate and Redeem APIs |
| 777         | Coupon code or ID is invalid                                                           | The provided coupon code or ID is invalid or not found.                                    | Validate and Redeem APIs |
| 777         | Validation is applied on metadata properties, but the key is not passed in the payload | The required metadata key for validation is missing in the payload.                        | Validate and Redeem APIs |
| 777         | User is not logged in and is attempting to validate the campaign user coupon           | The user must log in or sign up before redeeming the coupon code.                          | Validate and Redeem APIs |
| 777         | Validation for selected items failed                                                   | The discount validation for selected items has been failed.                                | Validate and Redeem APIs |
| 777         | OrderStatus contains wrong values                                                      | The order status provided is invalid; it should be one of pending, completed, or reverted. | Redeem API               |
| 777         | Duplicate `orderId` passed                                                             | The provided `orderId` has already been used for a previous transaction.                   | Redeem/Revert API        |
| 400         | Query parameter not passed                                                             | A required query parameter `query_param_key` is missing in the request.                    | Redeem/Revert API        |
| 400         | Query parameter is wrong                                                               | The query parameter type must be either `redeemed` or `reverted`.                          | Redeem/Revert API        |
| 777         | `order` and `item` object not passed                                                   | Both `order` and `item` objects are required in the request body.                          | Redeem/Revert API        |
| 777         | Mandatory keys in `item` or `order` object not passed                                  | A mandatory key is missing from the `order` or `item` object.                              | Redeem/Revert API        |
| 777         | `order` and `item` objects passed, but mandatory keys have empty values                | The mandatory key value in the `order` or `item` object cannot be empty.                   | Redeem/Revert API        |

# Partner Voucher API Errors

| Error Codes | Error Case                              | Error Behavior                                           | Applicable API    |
| ----------- | :-------------------------------------- | -------------------------------------------------------- | :---------------- |
| 777         | Identity is valid, vouchers assigned    | Returns all vouchers tied to the identity                | Fetch Voucher API |
| 400         | Identity is not passed                  | Returns HTTP 400 with `"error": "Invalid identity"`      | Fetch Voucher API |
| 777         | Identity is valid, no vouchers assigned | Returns empty `records` array with `"status": "success"` | Fetch Voucher API |

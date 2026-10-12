---
title: Reward Status API
excerpt: >-
  Learn how to use the Reward Status API to create or update a user's reward
  state with immediate read-after-write consistency.
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  robots: index
---
# Overview

The Reward Status API creates or updates a user's reward state. Use it to store business-defined coupon data, such as coupon lists or redemption status, with immediate read-after-write consistency. A value you write here is available to the [Fetch Reward API](doc:fetch-reward-api) as soon as the write succeeds.

CleverTap does not interpret or validate the meaning of the keys and values you send. You define your own reward model. This API only stores and returns it.

<Callout icon="📘" theme="info">
  ### Shared Endpoint

  The Reward Status API and the [Fetch Reward API](doc:fetch-reward-api) use the same URL. The HTTP method determines which operation runs. The `POST` method updates reward state, `GET` retrieves it.
</Callout>

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/rewards](https://in1.api.clevertap.com/1/promo/rewards)

## Region

To identify the API endpoint for your account's region, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

POST

# Query parameters

The following query parameters apply to this API:

| Parameter   | Description                                                                                                                                                                                                | Required/Optional | Data Type | Sample Value   |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | --------- | -------------- |
| `identity`  | The user identifier. Accepts an identity string, a phone number (with a `+` prefix), or an email address.                                                                                                  | Required          | String    | `user123`      |
| `namespace` | The reward bucket to write. Omit this parameter to write to the default bucket. For more information, refer to [Multiple Reward Buckets Handling](doc:reward-status-api#multiple-reward-buckets-handling). | Optional          | String    | `festive-sale` |

## Sample Request

The request body is a JSON object of business-defined keys and values describing the reward state you want to store. A value can be a string, number, boolean, array, or nested object.

Here is an example request to the Reward Status API, including headers needed to authenticate the request:

```curl cURL
curl -X POST "https://in1.api.clevertap.com/1/promo/rewards?identity=user123" \
  -H "X-CleverTap-Account-Id: <account_id>" \
  -H "X-CleverTap-Passcode: <passcode>" \
  -H "Content-Type: application/json" \
  -d '{
    "coupon_id": 1234,
    "active_coupons": "coupon1",
    "all_coupons": ["coupon1", "coupon2", "coupon3"],
    "activated_coupons": ["coupon1", "coupon2"],
    "redeemed_coupons": ["coupon5"],
    "all_coupons_ids": [1234, 5678, 9003]
  }'
```

<Callout icon="far fa-circle-info" theme="info">
  ### Supported Values Examples

  This sample request body illustrates every supported value type in one place. For example, a single number (`coupon_id`), a single string (`active_coupons`), an array of strings (`all_coupons`, `activated_coupons`, `redeemed_coupons`), and an array of numbers (`all_coupons_ids`).
</Callout>

# Body Parameters

The following table describes each body parameter of the sample request.

| Parameter           | Description                                                                                                                                                               | Required/Optional | Data Type                  | Sample Value                        |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | -------------------------- | ----------------------------------- |
| `coupon_id`         | Example key for a single numeric value.                                                                                                                                   | Optional          | Number                     | `1234`                              |
| `active_coupons`    | Example key for a single string value.                                                                                                                                    | Optional          | String                     | `"coupon1"`                         |
| `all_coupons`       | Example key listing every coupon assigned to the user.                                                                                                                    | Optional          | Array of String            | `["coupon1", "coupon2", "coupon3"]` |
| `activated_coupons` | Example key listing coupons the user has activated.                                                                                                                       | Optional          | Array of String            | `["coupon1", "coupon2"]`            |
| `redeemed_coupons`  | Example key listing coupons the user has redeemed. Accepts either a single string, for example `"coupon5"`, or an array of strings, for example `["coupon5", "coupon9"]`. | Optional          | String and Array of String | `["coupon5"]`                       |
| `all_coupons_ids`   | Example key listing numeric IDs for every coupon assigned to the user.                                                                                                    | Optional          | Array of Number            | `[1234, 5678, 9003]`                |

<Callout icon="📘" theme="info">
  ### Body Keys are Business-defined

  `coupon_id`, `active_coupons`, `all_coupons`, `activated_coupons`, `redeemed_coupons`, and `all_coupons_ids` are illustrative key names used in this sample request, not a fixed schema. Send any keys and values that describe your own reward model.
</Callout>

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the _Settings_ > _Project_ page on the CleverTap dashboard.


<Image src="https://files.readme.io/42c7e4ef1d09542be6811863b60f2190b4050c785f52740353f55e8434d1d4bf-image.png" alt="Find CleverTap Project Details" align="center" caption="Find CleverTap Project Details" border={true} />


## Sample Response

The following is a sample response for a successful update:

```json
{
  "status": "success",
  "records": [
    {
      "namespace": "default",
      "state": {
        "coupon_id": 1234,
        "active_coupons": "coupon1",
        "all_coupons": ["coupon1", "coupon2", "coupon3"],
        "activated_coupons": ["coupon1", "coupon2"],
        "redeemed_coupons": ["coupon5"],
        "all_coupons_ids": [1234, 5678, 9003]
      }
    }
  ]
}
```

The following is a sample response for a failed update.

```json
{
  "status": "fail",
  "error": "at least one key-value pair must be provided",
  "code": 738
}
```

<Callout icon="📘" theme="info">
  ### HTTP Status

  This API always returns HTTP 200. Check the `status` field in the response body to determine whether the request succeeded or failed.
</Callout>

## Response Fields

The following fields appear in the response:

| Field                 | Description                                                                                                                       | Data Type |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------- |
| `status`              | The status of the API response.                                                                                                   | String    |
| `records`             | On success, an array containing one object for the `namespace` that was written to.                                               | Array     |
| `records[].namespace` | The reward bucket the state was written to. `default` unless you passed a `namespace` query parameter.                            | String    |
| `records[].state`     | The complete, current state stored for this namespace, including keys this request did not touch.                                 | Object    |
| `error`               | Present only on failure. A human-readable description of what went wrong.                                                         | String    |
| `code`                | Present only on failure. For more information, refer to [Reward Status API Errors](doc:api-error-cases#reward-status-api-errors). | Integer   |

## Multiple Reward Buckets Handling

Most integrations only need one bucket per user. If you do not pass a `namespace`, everything you store for a user lives in a single `default` bucket, and every request automatically refers to it.

To keep more than one independent set of reward data for the same user, for example, coupons from two different campaigns tracked separately, pass your own `namespace` value.

The following is a sample request using a custom namespace.

```curl cURL
POST /1/promo/rewards?identity=user123&namespace=festive-sale
```

```json
{ "all_coupons": ["coupon10", "coupon11"], "redeemed_coupons": ["coupon10"] }
```

Any string works as a namespace name except `*`, that it is reserved for the Fetch Reward API's `every bucket` request. Sending `namespace = *` to this endpoint fails with an error code `745`.

## API Behavior

The following table describes how the Reward Status API behaves for every update case, including edge cases, with a sample request and response for each.

| Case                                    | Example                                                                                                                                                                                                                                      | Behavior                                                                                                                                                                             |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `identity` not passed                   | Request: `POST /1/promo/rewards` with no `identity` query parameter<br />Response: `{"status":"fail","error":"identity is required"}`                                                                                                        | Returns an error. `identity` is required on every request.                                                                                                                           |
| Create or update reward state           | Request: `{"all_coupons":["coupon1","coupon2","coupon3"],"active_coupons":["coupon1","coupon2"]}`<br />Response: `{"status":"success","state":{"all_coupons":["coupon1","coupon2","coupon3"],"active_coupons":["coupon1","coupon2"]}}`       | Returns success. The key-value pairs are stored for the identity.                                                                                                                    |
| Update an existing key with a new value | Stored: `{"active_coupons":["coupon1","coupon2"]}`<br />Request: `{"active_coupons":["coupon1","coupon2","coupon3"]}`<br />Response: `{"status":"success","state":{"active_coupons":["coupon1","coupon2","coupon3"]}}`                       | Returns success. The existing value for that key is replaced completely. It is not merged item-by-item.                                                                              |
| Remove values from an existing key      | Stored: `{"active_coupons":["coupon1","coupon2","coupon3"]}`<br />Request (to remove `coupon3`): `{"active_coupons":["coupon1","coupon2"]}`<br />Response: `{"status":"success","state":{"active_coupons":["coupon1","coupon2"]}}`           | Returns success. The key now stores only the values you sent.                                                                                                                        |
| Key not present in the request payload  | Stored: `{"all_coupons":["coupon1","coupon2"],"active_coupons":["coupon1"]}`<br />Request: `{"active_coupons":["coupon1"]}`<br />Response: `{"status":"success","state":{"all_coupons":["coupon1","coupon2"],"active_coupons":["coupon1"]}}` | Returns success. `all_coupons` is left untouched.                                                                                                                                    |
| Value type for a key                    | Request: `{"coupon_id":1234,"active_coupons":"coupon1","all_coupons_ids":[1234,5678,9003]}`<br />Response: `{"status":"success","state":{"coupon_id":1234,"active_coupons":"coupon1","all_coupons_ids":[1234,5678,9003]}}`                   | Returns success for a single number (`coupon_id`), a single string (`active_coupons`), an array of strings, or an array of numbers (`all_coupons_ids`).                              |
| Duplicate values within the same array  | Request: `{"active_coupons":["coupon1","coupon1","coupon2"]}`<br />Response: `{"status":"success","state":{"active_coupons":["coupon1","coupon1","coupon2"]}}`                                                                               | Returns success. Duplicate values are accepted. Uniqueness is not enforced.                                                                                                          |
| Empty array for a key                   | Stored: `{"active_coupons":["coupon1","coupon2","coupon3"]}`<br />Request: `{"active_coupons":[]}`<br />Response: `{"status":"success","state":{"active_coupons":[]}}`                                                                       | Returns success. All values under that key are cleared. The key itself is kept.                                                                                                      |
| Duplicate keys in the request body      | Request body sent with `active_coupons` twice, first as `["coupon1"]`, then as `["coupon1","coupon2"]`<br />Response: `{"status":"success","state":{"active_coupons":["coupon1","coupon2"]}}`                                                | Returns success. The last occurrence of a duplicate key is stored. Earlier occurrences in the same request are discarded.                                                            |
| Case sensitivity for keys and values    | Request: `{"redeemed_coupons":["coupon1"],"REDEEMED_COUPONS":["coupon2"]}`<br />Response: `{"status":"success","state":{"redeemed_coupons":["coupon1"],"REDEEMED_COUPONS":["coupon2"]}}`                                                     | Case sensitivity is not applied. `redeemed_coupons` and `REDEEMED_COUPONS` are treated as different keys and stored separately.                                                      |
| Empty request body                      | Request: `{}`<br />Response: `{"status":"fail","error":"at least one key-value pair must be provided"}`                                                                                                                                      | Returns an error. At least one key-value pair is required.                                                                                                                           |
| `null` value, at any nesting depth      | Request: `{"active_coupons":["coupon1",null]}`<br />Response: `{"status":"fail","error":"values must not be null"}`                                                                                                                          | Returns an error, regardless of how deeply the `null` is nested.                                                                                                                     |
| Empty string as a value                 | Request: `{"active_coupons":["","coupon1"]}`<br />Response: `{"status":"success","state":{"active_coupons":["","coupon1"]}}`                                                                                                                 | Returns success. Empty strings are accepted as valid values.                                                                                                                         |
| Multiple keys where one is invalid      | Request: `{"active_coupons":["coupon1","coupon2"],"coupon_id":null}`<br />Response: `{"status":"fail","error":"values must not be null"}`                                                                                                    | Returns the error for the invalid key. The entire request fails. No partial updates are made, including for the other, valid keys in the same request, such as `active_coupons`.     |
| Special characters in keys or values    | Request: `{"coupon@123":["₹100OFF"]}`<br />Response: `{"status":"success","state":{"coupon@123":["₹100OFF"]}}`                                                                                                                               | Returns success. Special characters are accepted and stored as provided.                                                                                                             |
| Array order                             | Request: `{"active_coupons":["coupon2","coupon1"]}`<br />Response: `{"status":"success","state":{"active_coupons":["coupon2","coupon1"]}}`                                                                                                   | Values are stored and returned in the exact order you sent them.                                                                                                                     |
| Stored history                          | Request 1: `{"redeemed_coupons":["coupon5"]}`<br />Request 2 (sent later): `{"redeemed_coupons":["coupon9"]}`<br />Fetch response: `{"status":"success","state":{"redeemed_coupons":["coupon9"]}}`                                           | Only the latest state is stored. Each update fully replaces the value of the keys it touches. The `["coupon5"]` value from Request 1 is not retained or retrievable after Request 2. |
| Limits                                  | Request: `{"active_coupons":["coupon1","coupon2", ...,"coupon5000"]}` (an array of 5,000 values)<br />Response: `{"status":"success","state":{"active_coupons":["coupon1","coupon2", ...,"coupon5000"]}}`                                    | There is no limit on the number of keys per user, the number of values in an array, or string length. A payload this size is accepted the same as a small one.                       |

<Callout icon="📘" theme="warn">
  ### Failure Handling

  If a request updates several keys and any one of them fails validation, the entire request fails. No partial updates are made.
</Callout>

# API Errors

The following are the custom API errors you may encounter when requests to the Reward Status APIs fail:

| Error Codes | Error Case                               | Error Details                                                                                                     |
| ----------- | :--------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| 400         | Identity is not passed                   | Returns HTTP 400 with `"error": "identity is required"`.                                                          |
| 738         | Request body has no key-value pairs      | The request body is `{}`. At least one key-value pair is required.                                                |
| 741         | Request body is not valid JSON           | The request body is malformed, or the top-level JSON value is not an object.                                      |
| 740         | `null` value in the request body         | A value is `null`, at any nesting depth, including inside an array or a nested object.                            |
| 742         | `namespace` exceeds maximum length       | `namespace` is longer than 64 characters.                                                                         |
| 743         | `identity` exceeds maximum length        | `identity` is longer than 255 characters.                                                                         |
| 744         | Stray control character in key or value  | A key or value in the request body contains a stray control character.                                            |
| 745         | Reserved `namespace` value used on write | `namespace = *` was passed on a write request. `*` is reserved for the Fetch Reward API's "every bucket" request. |
| 777         | An unexpected error occurs               | The request failed due to an unexpected server-side error not covered by any of the cases above.                  |

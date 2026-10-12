---
title: Fetch Reward API
excerpt: >-
  Learn how to use the Fetch Reward API to retrieve a user's reward state with
  immediate read-after-write consistency.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

The Fetch Reward API retrieves the reward state previously stored for a user with the [Reward Status API](doc:reward-status-api). The response returns exactly the latest stored state for the requested identity, with immediate read-after-write consistency. A value written with the Reward Status API is available here as soon as the write succeeds.

<Callout icon="📘" theme="info">
  ### Shared endpoint

  The Fetch Reward API and the [Reward Status API](doc:reward-status-api) use the same URL. The HTTP method determines which operation runs. The `GET` method retrieves reward state, and `POST` updates it.
</Callout>

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/promo/rewards](https://in1.api.clevertap.com/1/promo/rewards)

## Region

To identify the API endpoint for your account's region, refer to [Region](doc:common-api-components#region).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](doc:common-api-components#headers).

# HTTP Method

GET

# Query parameters

The following query parameters apply to this API:

| Parameter   | Description                                                                                                                                                                                           | Required/Optional | Data Type | Sample Value   |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | --------- | -------------- |
| `identity`  | The user identifier. Accepts an identity string, a phone number (with a `+` prefix), or an email address.                                                                                             | Required          | String    | `user123`      |
| `namespace` | The reward bucket to read. Omit this parameter to read the default bucket. For more information, refer to [Multiple Reward Buckets Handling](doc:reward-status-api#multiple-reward-buckets-handling). | Optional          | String    | `festive-sale` |

## Sample Request

Here's the example request to the Fetch Reward API, including the header needed to authenticate the request:

```bash
curl -X GET "https://<region>.api.clevertap.com/1/promo/rewards?identity=user123" \
  -H "X-CleverTap-Account-Id: <account_id>" \
  -H "X-CleverTap-Passcode: <passcode>"
```

## Find CleverTap Account ID and Passcode

You can find the CleverTap Account ID and Passcode from the CleverTap dashboard by navigating to the _Settings_ > _Project_ page on the CleverTap dashboard.


<Image src="https://files.readme.io/42c7e4ef1d09542be6811863b60f2190b4050c785f52740353f55e8434d1d4bf-image.png" alt="Find CleverTap Project Details" align="center" caption="Find CleverTap Project Details" border={true} />


## Sample Response

The following is a sample response when a reward state has been stored for the identity:

```json
{
  "status": "success",
  "records": [
    {
      "namespace": "default",
      "state": {
        "all_coupons": ["coupon1", "coupon2", "coupon3"],
        "activated_coupons": ["coupon1", "coupon2"]
      }
    }
  ]
}
```

The following is a sample response when nothing has been stored yet for the identity:

```json
{
  "status": "success",
  "records": []
}
```

<Callout icon="📘" theme="info">
  ### No Error for an Identity with no Stored State

  If nothing has been stored yet for the identity you pass, this API still returns a success response, with an empty `records` array. It does not return an error.
</Callout>

The following is a sample response for a failed request.

```json
{
  "status": "fail",
  "error": "identity is required",
  "code": 400
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
| `records`             | On success, an array of stored `namespace` documents. Empty if nothing has been stored for this identity.                         | Array     |
| `records[].namespace` | The reward bucket this record belongs to.                                                                                         | String    |
| `records[].state`     | The stored state for this namespace. A key with no values stored is returned as an empty array, not omitted.                      | Object    |
| `error`               | Present only on failure. A human-readable description of what went wrong.                                                         | String    |
| `code`                | Present only on failure. For more information, refer to [Reward Status API Errors](doc:api-error-cases#reward-status-api-errors). | Integer   |

## Multiple Reward Buckets Handling

Fetching without a `namespace` returns only the `default` bucket, not every bucket you have created for that identity.

The following is a sample request and response for the default bucket:

```
GET /1/promo/rewards?identity=user123
```

```json
{
  "status": "success",
  "records": [
    { "namespace": "default", "state": { "all_coupons": ["coupon1", "coupon2"] } }
  ]
}
```

To fetch every bucket stored for a user in one response, pass the reserved value `namespace = *`.

```
GET /1/promo/rewards?identity=user123&namespace=*
```

```json
{
  "status": "success",
  "records": [
    { "namespace": "default", "state": { "all_coupons": ["coupon1", "coupon2"] } },
    { "namespace": "festive-sale", "state": { "all_coupons": ["coupon10", "coupon11"], "redeemed_coupons": ["coupon10"] } }
  ]
}
```

`*` is reserved for this purpose on Fetch. Sending `namespace = *` on the [Reward Status API](doc:reward-status-api) fails with an error code `745`.

## API Behavior

Array order preservation and the no-history/latest-state-only guarantee work the same way for this API as they do for the Reward Status API. For more information, refer to [API Behavior](doc:reward-status-api#api-behavior).

# Errors

To know about the errors applicable to the Fetch Reward API, refer to [Reward Status API Errors](doc:api-error-cases#reward-status-api-errors).

<br />

---
title: Update Segment API
excerpt: >-
  Learn how to manage user segments in CleverTap with API for seamless audience
  targeting and engagement.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The Update Segment API allows you to manage user profiles in CleverTap by adding or removing users from API segments. It does not apply to segments created via the dashboard or partner segments. This API allows bulk updates to user segments, ensuring profiles stay accurate in the relevant segments based on updated behavior, preferences, or attributes.

The Update Segment API uses partial processing. If a request contains both valid and invalid entries, valid entries are processed while invalid entries are skipped. Invalid entries are logged in the Error Stream dashboard, along with Segment ID, Customer Segment ID, and User Identity.

With this API, you can:

* Update multiple user profiles simultaneously, adding or removing them from various segments.
* Refine segments based on changing user data or actions.
* Manage and maintain dynamic user segments for better targeting and engagement.
* Simplify user segmentation by using either CleverTap segment IDs, `ct_segment_id`, or `customer-defined segment IDs`, `customer_segment_id`.

# Base URL

Here is an example base URL from the account in the India region:

`https://<region>.api.clevertap.com/api/updateSegment`

## Region

To identify the API endpoint for your account's region, refer to [Region](https://developer.clevertap.com/docs/common-api-components#regions).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

# HTTP Method

POST

# Request Parameters

The request body contains an array of `bulk_profiles`, where each entry consists of user profile details and segment information.

| Parameter             | Description                                                                            | Required/Optional                             | Type    | Sample Value      |
| --------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------- | ------- | ----------------- |
| `bulk_profiles`       | A list containing multiple user profile updates in bulk.                               | Required                                      | Array   |                   |
| `user_profiles`       | A list of user identity data specifying either `guid` or `identity`.                   | Required                                      | Array   |                   |
| `guid`                | A unique identifier for an anonymous user. Checked only if `identity` is not provided. | Optional if `identity` is present.            | String  | `abcde12345`      |
| `identity`            | The CleverTap identity of the user. Checked only if `guid` is not present.             | Optional if `guid` is present.                | String  | `CT123456`        |
| `segments`            | A list of segments to which the user profiles must be added or removed.                | Required                                      | Array   |                   |
| `ct_segment_id`       | The internal CleverTap segment ID.                                                     | Optional if `customer_segment_id` is present. | Integer | `1234`            |
| `customer_segment_id` | A unique identifier for a customer-defined segment.                                    | Optional if `ct_segment_id` is present.       | String  | `abc123`          |
| `action`              | Defines whether the user should be added or removed from the segment.                  | Required                                      | String  | `ADD` or `REMOVE` |

Each `bulk_profiles` entry updates multiple user profiles and assigns them to specified segments using either a CleverTap segment ID (`ct_segment_id`) or a customer-defined segment ID (`customer_segment_id`). Each request can include up to 100 user IDs for one or more segments and up to 100 segments.

> 📘 Request Payload
>
> If both `ct_segment_id` and `customer_segment_id` are provided in the request payload, they must match. If they do not match, the corresponding entry is skipped, and the error is logged in the Error Stream dashboard. Other valid entries in the request are still being processed.

# Example Request

The following is the sample request:

```json
{
  "bulk_profiles": [
    {
      "user_profiles": [
        {
          "guid": "abcde12345",
          "identity": "abc@gmail.com"
        },
        {
          "guid": "fghij67890",
          "identity": "xyz@gmail.com"
        }
      ],
      "segments": [
        {
          "ct_segment_id": 1234,
          "customer_segment_id": "abc123",
          "action": "ADD"
        },
        {
          "ct_segment_id": 5678,
          "action": "REMOVE"
        },
        {
          "customer_segment_id": "customer_id_123",
          "action": "ADD"
        }
      ]
    }
  ]
}
```

# Example Response

The API returns HTTP 200 when the request is accepted, even if some entries are invalid. Valid entries are processed successfully, while invalid entries are skipped and reported in the Error Stream dashboard.

The following is the sample response:

```json
{
  "success": true
}
```

# Request Limit

For information on request limits for the Update Segment API, refer to [API Request Limit](doc:api-request-limit#concurrent-request-limits-for-apis).

# Error Stream Reporting

Invalid entries in the request are logged to the Error Stream dashboard instead of causing the entire request to fail.

Each logged error includes the following fields:

| Field               | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| Segment ID          | The CleverTap segment ID (`ct_segment_id`), if provided.         |
| Customer Segment ID | The customer-defined segment identifier (`customer_segment_id`). |
| User Identity       | The identity or GUID of the user associated with the request.    |

You can view these errors in the Error Stream dashboard to diagnose issues with individual segment updates.

# Errors

The following errors may occur while processing _Update Segment API_ requests.

## Request-Level Errors

Some errors apply to the entire request, while others are logged in the Error Stream dashboard when individual entries are invalid.

| Error Codes | Error Case                                | Error Description                                                                     |
| :---------- | :---------------------------------------- | ------------------------------------------------------------------------------------- |
| 401         | Authentication Failure                    | User authentication failed. Ensure all authentication values are entered and correct. |
| 400         | Incorrect Request Parameters              | The request parameters are invalid. Verify the parameters and try again.              |
| 100         | Maximum number of profile updates reached | A maximum of 100 profile updates can be included in one request.                      |
| 100         | No user profiles are provided             | No user profiles were provided in the request. Add profiles and try again.            |
| 100         | Segment limit exceeded                    | More than 100 segments were added in the request.                                     |
| 100         | Profile limit exceeded                    | More than 100 user IDs added for one or more segments.                                |
| 500         | Unknown cause                             | The request could not be processed due to a system error. Try again.                  |

## Segment Processing Errors

The following errors are logged in the Error Stream dashboard when specific request entries are invalid. These errors do not cause the entire request to fail. The affected entries are skipped while valid entries are processed.

| Error Type                       | Description                                                                              |
| -------------------------------- | ---------------------------------------------------------------------------------------- |
| API_SEGMENT_NOT_FOUND_OR_DELETED | The specified segment does not exist or has been deleted.                                |
| API_SEGMENT_ID_MISMATCH          | The provided `ct_segment_id` and `customer_segment_id` do not refer to the same segment. |
| API_SEGMENT_INVALID_ACTION       | The `action` value is invalid. Only `ADD` or `REMOVE` are supported.                     |
| API_SEGMENT_EMPTY_IDENTITY       | The user identity or GUID is missing or empty.                                           |
| API_SEGMENT_NO_SEGMENT_ID        | Neither `ct_segment_id` nor `customer_segment_id` was provided in the segment entry.     |

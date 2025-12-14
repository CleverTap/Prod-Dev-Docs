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

The Update Segment API allows you to manage and update user profiles within CleverTap by adding or removing users from API segments only. It does not apply to segments created via the dashboard or partner segments. This API allows bulk updates to user segments, ensuring profiles stay accurate in the relevant segments based on updated behavior, preferences, or attributes.

With this API, you can:

* Update multiple user profiles simultaneously, adding or removing them from various segments.
* Refine segments based on changing user data or actions.
* Manage and maintain dynamic user segments for better targeting and engagement.
* Simplify the user segmentation process by using either CleverTap segment IDs that is, `ct_segment_id` or customer-defined segment IDs, that is, `customer_segment_id`.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/api/updateSegment](https://in1.api.clevertap.com/api/updateSegment)

### Region

To identify the API endpoint for the region of your account, refer to [Region](https://developer.clevertap.com/docs/common-api-components#regions).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

# HTTP Method

POST

# Body Parameters

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

Each `bulk_profiles` entry updates multiple user profiles and assigns them to specified segments using either a CleverTap segment ID (`ct_segment_id`) or a customer-defined segment ID (`customer_segment_id`).

> 📘 Request Payload
>
> If both `ct_segment_id` and `customer_segment_id` are provided in the request payload, they must match.  If they do not match, the segment update request will fail.

### Sample Request

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
          "ct_segment_id": "1234",
          "customer_segment_id": "abc123",
          "action": "ADD"
        },
        {
          "ct_segment_id": "5678",
          "action": "Remove"
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

### Sample Response

```json
{
  "success": true
}
```

# Errors

The following are the API errors that you may receive upon failure requests of the Update Segments API:

| Error Codes | Error Case                                | Error Description                                                                           |
| :---------- | :---------------------------------------- | ------------------------------------------------------------------------------------------- |
| 401         | Authentication Failure                    | User authentication failed. Ensure all authentication values are entered and correct.       |
| 400         | Incorrect Request Parameters              | Parameters entered for uploading profiles are invalid. Verify the parameters and try again. |
| 100         | Maximum number of profile updates reached | A maximum of 100 profiles can be uploaded in one request.                                   |
| 100         | No user profiles are provided             | No profiles are available for upload. Add profiles and try again.                           |
| 500         | Unknown cause                             | The request could not be processed due to a system error. Try again.                        |

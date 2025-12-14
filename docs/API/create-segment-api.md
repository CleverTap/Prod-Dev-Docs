---
title: Create Segment API
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

The Create Segment API allows you to create new user segments within CleverTap, enabling personalized and targeted engagement strategies. The API automates the creation of user segments, streamlining your efforts to improve user engagement and retention. 

This API is useful for:

- Creating dynamic user groups for targeted marketing or messaging campaigns.
- Enhancing the segment creation process via APIs to scale marketing efforts across multiple channels.

# Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/api/createSegment>

### Region

To identify the API endpoint for the region of your account, refer to [Region](https://developer.clevertap.com/docs/common-api-components#regions).

# Headers

For more information about API headers used while processing API requests, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

# HTTP Method

POST

# Body Parameters

The request body contains an array of customer segments, each defined by a `segment_name` and a `customer_segment_id`. Each object in the `segments` array represents a customer segment with a unique identifier.

| Parameter             | Description                                                                     | Required/Optional | Type   | Sample Value    |
| --------------------- | ------------------------------------------------------------------------------- | ----------------- | ------ | --------------- |
| `segment_name`        | The name of the segment. It must be unique within the request.                  | Required          | String | `Premium Users` |
| `customer_segment_id` | A unique identifier for the segment. It must be unique across all API requests. | Required          | String | `SEG12345`      |

> 📘 Naming Conventions for Creating API Segment
> 
> To optimize the usage of the Create Segment API, follow these best practices:
> 
> - Use clear, consistent naming conventions for segment names. For example, Premium Users 2024, Active Buyers.
> - Avoid special characters or spaces in segment names.
> - Maintain a structured approach to naming to easily track and reference segments.

## Sample Request

```json
[
    {
      "segment_name": "Premium Users",
      "customer_segment_id": "SEG12345"
    },{
      "segment_name": "Frequent Buyers",
      "customer_segment_id": "SEG67890"
    }
  ]
```

## Sample Response

```json
{
  Segments: [
  	{
    	"segment_name": "Premium Users",
    	"customer_segment_id": "SEG12345",
    	"id": 123
},
    {
    	"segment_name": "Frequent Buyers",
    	"customer_segment_id": "SEG67890",
    	"id": 456
  	]
}
```

# Errors

The following are the API errors that you may encounter upon failure requests of the Create Segments API:

| Error Codes | Error Case                                           | Error Description                                                                                                                                                                                                                                                |
| ----------- | :--------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 401         | Authentication Failure                               | User authentication failed. Ensure all values are entered and correct.                                                                                                                                                                                           |
| 400         | Incorrect request Parameters                         | Parameters entered for segment creation are invalid. Verify the parameters and try again.                                                                                                                                                                        |
| 400         | Maximum number of profile updates reached            | A maximum of 100 segments can be created in one request.                                                                                                                                                                                                         |
| 403         | The request contains more than the allowed segments. | You have already created segments allowed by your plan. For example, if your plan allows only 100 segment creations and 97 segments already exist, only three more segments can be created. If the request contains more than three segments, this error occurs. |
| 201         | Error saving a segment to the CleverTap dashboard.   | Segment creation failed. Try again.                                                                                                                                                                                                                              |
| 500         | Unknown cause                                        | The request could not be processed due to a system error. Try again.                                                                                                                                                                                             |
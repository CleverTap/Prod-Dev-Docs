---
title: API Request Limit
excerpt: Understand the concurrent request limit for CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Concurrent Requests

Concurrent requests are the maximum number of parallel API requests you can send to CleverTap. Think of this as the system's maximum capacity to handle multiple requests from users simultaneously without causing delays or failures.

When interacting with CleverTap APIs, you might need to perform several operations simultaneously, such as uploading device tokens, retrieving campaign data, or managing user profiles. Each operation is an API request. CleverTap limits concurrent API requests per user account to ensure smooth and efficient processing and maintain service performance and stability.

Here is a practical example to help you understand concurrent requests:

**Uploading Device Tokens:**Suppose you have a batch of device tokens to upload. The concurrent request limit for this operation is 15, meaning you can send up to 15 upload requests at the same time. If you exceed this limit, the system rejects the extra requests, and you receive an** HTTP 429** error with the message _Too many concurrent requests_.

# Importance of Concurrent Request Limits

- **Performance:**  Limits ensure the system handles requests efficiently without overloading, providing consistent and reliable performance.
- **Fair Usage:** Prevents one account from using all the available resources.
- **Stability:**  Maintains overall service stability and availability by managing the load.

Understanding concurrent request limits helps you manage your API usage, avoid exceeding limits, and ensure smooth application performance.

# Concurrent Request Limits for APIs

The following table lists the maximum number of concurrent requests allowed per user account for an API:

> 📘 Note
> 
> CleverTap does not enforce rate limits based on specific time frames (minute, hour, or day), making our APIs more accessible.



[block:parameters]
{
  "data": {
    "h-0": "API",
    "h-1": "Maximum  \nConcurrent Requests",
    "0-0": "[Catalog API](doc:catalog-api-endpoints) ",
    "0-1": "3",
    "1-0": "[Create Campaign](doc:get-campaigns-api) ",
    "1-1": "3",
    "2-0": "[Custom List API](doc:custom-list-endpoints) ",
    "2-1": "3",
    "3-0": "[Delete User Profile](doc:delete-user-profile-api) ",
    "3-1": "3",
    "4-0": "[Demerge User Profile](doc:demerge-user-profile-api) ",
    "4-1": "3",
    "5-0": "[External Trigger API](https://developer.clevertap.com/docs/external-trigger-api) ",
    "5-1": "3",
    "6-0": "[Get Campaign Report](doc:get-campaign-report-api) ",
    "6-1": "3",
    "7-0": "[Get Campaigns](doc:get-campaigns-api) ",
    "7-1": "3",
    "8-0": "[Get Event Count](doc:get-event-count-api) ",
    "8-1": "3",
    "9-0": "[Get Events](doc:get-events-api) ",
    "9-1": "3",
    "10-0": "[Get Message Reports](doc:get-message-reports-api) ",
    "10-1": "3",
    "11-0": "[Get Profile Count](doc:get-profile-count-api) ",
    "11-1": "3",
    "12-0": "[Get User Profiles](doc:get-user-profiles-api) ",
    "12-1": "3",
    "13-0": "[Raise a Bulletin](doc:bulletins-api) ",
    "13-1": "3",
    "14-0": "[Real-Time Counts](doc:real-time-counts-api) ",
    "14-1": "3",
    "15-0": "[Stop Scheduled Campaign](doc:stop-campaign-api) ",
    "15-1": "3",
    "16-0": "[Top Property Counts](doc:top-property-counts-api) ",
    "16-1": "3",
    "17-0": "[Trends](doc:trends-api) ",
    "17-1": "3",
    "18-0": "[Upload Device Tokens](doc:upload-device-tokens-api) ",
    "18-1": "15",
    "19-0": "[Upload Events](doc:upload-events-api) ",
    "19-1": "15",
    "20-0": "[Upload User Profiles](doc:upload-user-profiles-api) ",
    "20-1": "15"
  },
  "cols": 2,
  "rows": 21,
  "align": [
    "left",
    "left"
  ]
}
[/block]
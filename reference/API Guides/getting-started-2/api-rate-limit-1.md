---
title: API Request Limit
excerpt: Understand the rate limit rules for CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Concurrent API Request Limit

The concurrent API request limit defines the maximum number of API calls an application or user can make simultaneously. The concurrency limits are defined per user account for each API. 

## Account-Level Request Limit

The following table lists the maximum number of concurrent requests allowed per user account for an API. The HTTP **429** error code will return a *Too many concurrent requests* error message upon breaching the concurrency limit. For more details on errors, refer to [API Errors](https://developer.clevertap.com/docs/api-errors#4xx-client-error-codes).

| API                                                  | Maximum Concurrent Requests |
| :--------------------------------------------------- | :-------------------------- |
| [Upload Device Tokens](doc:upload-device-tokens-api) | 15                          |
| [Upload Events](doc:upload-events-api)               | 15                          |
| [Upload User Profiles](doc:upload-user-profiles-api) | 15                          |
| [Get Campaigns](doc:get-campaigns-api)               | 3                           |
| [Stop Scheduled Campaign](doc:stop-campaign-api)     | 3                           |
| [Create Campaign](doc:get-campaigns-api)             | 3                           |
| [Get Campaign Report](doc:get-campaign-report-api)   | 3                           |
| [Real-Time Counts](doc:real-time-counts-api)         | 3                           |
| [Get Message Reports](doc:get-message-reports-api)   | 3                           |
| [Get Events](doc:get-events-api)                     | 3                           |
| [Get Event Count](doc:get-event-count-api)           | 3                           |
| [Get User Profiles](doc:get-user-profiles-api)       | 3                           |
| [Get Profile Count](doc:get-profile-count-api)       | 3                           |
| [Demerge User Profile](doc:demerge-user-profile-api) | 3                           |
| [Delete User Profile](doc:delete-user-profile-api)   | 3                           |
| [Trends](doc:trends-api)                             | 3                           |
| [Top Property Counts](doc:top-property-counts-api)   | 3                           |
| [Raise a Bulletin](doc:bulletins-api)                | 3                           |
| [Catalog API](doc:catalog-api-endpoints)             | 3                           |
| [Custom List API](doc:custom-list-endpoints)         | 3                           |

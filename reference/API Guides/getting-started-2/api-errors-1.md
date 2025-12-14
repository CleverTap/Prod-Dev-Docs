---
title: API Errors
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
If your CleverTap API request fails, you will receive an error response including a response code and a message explaining the reason for the error. 


## Generic Error Codes

The following response code pattern applies across CleverTap API requests:
## 2xx Success Codes

2xx codes indicate that the request was successfully processed. The following are the most common 2xx status codes:

| Status Code | Text | Description |
| - | - | - |
| 200 | OK | The request has been successfully processed. |
| 201 | Created | The request has been fulfilled and has resulted in one or more new resources being created. |
| 202 | Accepted | The request has been accepted for processing, but the processing has not been completed. |
| 204 | No Content | The request has been successfully fulfilled by the server and there is no additional content to send in the response payload body. |

## 4xx Client Error Codes

4xx codes indicate that there was an error in either the request or the data. The following are the most common 4xx status codes:

| Status Code | Text | Description |
| - | - | - |
| 400 | Bad Request | Invalid parameter passed |
| 401 | Unauthorized | Invalid credentials or URI |
| 403 | Forbidden | Operation not allowed |
| 404 | Not Found | Resource does not exist or cannot be found |
| 409 | Conflict | Resource already exists |
| 429 | Too Many Requests | Rate limit exceeded |

## 5xx Server Error Codes

5xx codes indicate that there was an internal error with the server. The following are the most common 5xx status codes:

| Status Code | Text | Description |
| - | - | - |
| 500 | Internal Server Error | Error within the API  |
| 503 | Service Unavailable | System is unavailable. Try again later. |


## API Specific Error Codes

## [Upload Events](https://developer.clevertap.com/docs/upload-events-api) API
  * 509 - Event name mandatory
  * 512 - Invalid event structure
  * 513 - Raised a restricted system event
  * 522 - Event properties limit exceeded
  * 523 - Missing identity
  * 524 - Data neither event nor profile
  * 525 - Timestamp not in UNIX epoch format
  * 526 - objectId invalid
  * 528 - Discarded event is raised.
  * 555 - Miscellaneous server error

## [Upload User Profiles](https://developer.clevertap.com/docs/upload-user-profiles-api) API
  * 514 - Gender invalid
  * 515 - Email invalid
  * 516 - Phone number invalid
  * 517 - Employment status invalid
  * 518 -  Education status invalid
  * 519 - Marital status invalid
  * 520 - Age invalid
  * 523 - Missing identity
  * 524 - Data neither event nor profile
  * 525 - Timestamp not an epoch
  * 526 - objectId invalid
  * 555 - Miscellaneous server error

## [Upload Device Tokens](https://developer.clevertap.com/docs/upload-device-tokens-api) API
  * 530 - Invalid ‘objectId’
  * 531 - Invalid token data
  * 532 - Token ‘id’ is not present
  * 533 - Token ‘type’ is not present
  * 534 - Token ‘type’ is not valid for ‘objectId’
  * 535 - Invalid token data
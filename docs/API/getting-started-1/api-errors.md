---
title: Errors
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

The following response code pattern applies across API requests:
  * 200 - Successful Request
  * 400 - Bad Request. Some mandatory parameter was missing.
  * 429 - Too many requests were made concurrently. Three concurrent requests are the max. Please try again after 10 minutes.
  * 500 - Server Error
  * 503 - Service Unavailable. Please retry later.

We also have endpoint-specific errors that are documented below. 

# Upload Events API
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

# Uploads User Profiles API
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

# Upload Device Tokens API
  * 530 - Invalid ‘objectId’
  * 531 - Invalid token data
  * 532 - Token ‘id’ is not present
  * 533 - Token ‘type’ is not present
  * 534 - Token ‘type’ is not valid for ‘objectId’
  * 535 - Invalid token data
---
title: Get User Profiles API
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
The Get User Profiles API lets you download user profiles from CleverTap. 

There are two ways to get user profiles. The first way is to download user profiles that have performed a specific event. For example, you can download a list of users that have launched your app in the past day. The second way to download user profiles is by requesting the specific users needed.

# Get User Profiles by Events

Getting a list of user profiles by events requires two steps:
  * With your first API call you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of user profile records in step 2. 
  * You will make a second API call using the cursor from step 1. This call will return the first batch of user profiles and and another cursor to get the second batch of user profiles. You will repeat this process until the cursor is not returned in the response, which means there are no more matching user profiles.

## Step 1: Get Cursor by Specifying Events for User Profiles

In this request, you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of user profiles in the step 2.

### Base URL
https://api.clevertap.com/1/profiles.json

### HTTP Method
POST

### Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### URL Parameter
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "0-0": "batch_size",
    "0-1": "The number of results to return in each API call. The maximum is 5000.",
    "0-2": "int",
    "0-3": "50",
    "h-3": "Example Value",
    "h-2": "Type"
  },
  "cols": 4,
  "rows": 1
}
[/block]
### Body Parameters
[block:parameters]
{
  "data": {
    "0-0": "event_name",
    "1-0": "from",
    "2-0": "to",
    "h-0": "Parameter",
    "0-1": "The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited.",
    "1-1": "Start of date range for events needed. Value specified in format YYYYMMDD.",
    "2-1": "End of date range for events needed. Value specified in format YYYYMMDD.",
    "h-2": "Type",
    "h-1": "Description",
    "0-2": "string",
    "1-2": "int",
    "2-2": "int",
    "2-3": "20171225",
    "1-3": "20171201",
    "0-3": "“App Launched”",
    "h-3": "Example Value"
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}' \"https://api.clevertap.com/1/profiles.json?batch_size=50\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"\n",
      "language": "curl"
    }
  ]
}
[/block]
### Example Response

[block:code]
{
  "codes": [
    {
      "code": "{\n \"cursor\":\"AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D\",\n \"status\":\"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
## Step 2: Get User Profiles Using Cursor

Using the cursor from step 1, you will now make an API call that will return the first batch of user profiles and and another cursor to get the second batch of user profiles. You will repeat this process until a cursor is not returned in the response, which means there are no more matching user profiles.

### Base URL
https://api.clevertap.com/1/profiles.json

### HTTP Method
GET

### Headers
The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "2-3": "\"Content-Type: application/json\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### URL Parameter

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "cursor",
    "0-1": "Required parameter. Specifies what set of user profiles to return.",
    "0-2": "string",
    "0-3": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6"
  },
  "cols": 4,
  "rows": 1
}
[/block]
### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://api.clevertap.com/1/profiles.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    }
  ]
}
[/block]
### Example Response
If there are more user profiles available, next_cursor will be returned. The value for next_cursor is used to get the next batch of user profiles. 

User profiles will be returned in a records object. The schema for the user profile object is documented in the section below.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"previous_cursor\": \"ZiZiNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg==\",\n  \"next_cursor\": \"ZiZjNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg%3D%3D\",\n  \"records\": [\n    {\n      \"identity\": \"5555555555\",\n      \"profileData\": {\n        \"favoriteFood\": \"pizza\"\n      },\n      \"platformInfo\": [\n        {\n          \"platform\": \"iOS\",\n          \"os_version\": \"10.2\",\n          \"app_version\": \"6.1.3\",\n          \"make\": \"Apple\",\n          \"model\": \"iPhone7,2\",\n          \"push_token\": \"95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3\",\n          \"objectId\": \"-1a063854f83a4c6484285039ecff87cb\"\n        },\n        {\n          \"platform\": \"Web\",\n          \"objectId\": \"a8ffcbc9-a747-4ee3-a791-c5e58ad03097\"\n        }\n      ]\n    }\n  ]\n}\n",
      "language": "json"
    }
  ]
}
[/block]
If there are no more user profiles available, next_cursor will not be returned and a success response will be returned.
[block:code]
{
  "codes": [
    {
      "code": "{ \"status\" : \"success\"}",
      "language": "json"
    }
  ]
}
[/block]
## User Profile Object

Here is the schema for the User Profile object.
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Description",
    "0-0": "email",
    "1-0": "profileData",
    "2-0": "identity",
    "3-0": "name",
    "4-0": "phone",
    "5-0": "platformInfo",
    "0-1": "User’s email address.",
    "1-1": "User’s custom profile properties.",
    "2-1": "Custom user identity provided by you.",
    "3-1": "User’s name.",
    "4-1": "User’s phone.",
    "5-1": "Array containing all platform names with the corresponding objectId (CleverTap ID), push_token, app_version, os_version, make (device make), model (device model) if available."
  },
  "cols": 2,
  "rows": 6
}
[/block]
# Download User Profiles by ID

You can download user profiles by requesting the specific user needed.

### Overview
You can retrieve a user’s profile and all the events performed by a specific user. Supported identity values are email, a custom identity, or the CleverTap objectId.

### Base URL
https://api.clevertap.com/1/profile.json

### HTTP Method
GET

### Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### URL Parameters
One of the three parameters documented below needs to be included in your request to specify the user profile needed.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "email",
    "1-0": "identity",
    "2-0": "objectId",
    "0-1": "User’s email address.",
    "1-1": "Custom user identity defined by you.",
    "2-1": "CleverTap ID.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "“jack@gmail.com”",
    "1-3": "“5555555555”",
    "2-3": "“1a063854f83a4c6484285039ecff87cb”"
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://api.clevertap.com/1/profile.json?email=jack@gmail.com\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    }
  ]
}
[/block]
### Example Response
The User profile will be returned in a record object. The schema for the user profile object is documented below.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"record\": {\n    \"email\": \"jack@gmail.com\",\n    \"profileData\": {\n      \"Last Score\": 308,\n      \"High Score\": 308,\n      \"Replayed\": true\n    },\n    \"events\": {\n      \"App Launched\": {\n        \"count\": 10,\n        \"first_seen\": 1457271567,\n        \"last_seen\": 1458041215\n      },\n      \"Charged\": {\n        \"count\": 6,\n        \"first_seen\": 1457962417,\n        \"last_seen\": 1458041276\n      }\n    },\n    \"platformInfo\": [\n      {\n        \"platform\": \"iOS\",\n        \"os_version\": \"10.2\",\n        \"app_version\": \"6.1.3\",\n        \"make\": \"Apple\",\n        \"model\": \"iPhone7,2\",\n        \"push_token\": \"95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3\",\n        \"objectId\": \"-1a063854f83a4c6484285039ecff87cb\"\n      },\n      {\n        \"platform\": \"Web\",\n        \"objectId\": \"a8ffcbc9-a747-4ee3-a791-c5e58ad03097\"\n      }\n    ]\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Additional Notes
  * API errors are described on [this page](doc:api-errors).
  * Requests are limited to 3 concurrent requests. If you exceed this limit, we will return a 429 error. Please try again after 10 minutes.

# Next Steps
Now that you’ve learned how to get user profiles from the CleverTap API, learn how to upload user profiles using the [Upload User Profiles API](doc:upload-user-profiles-api).
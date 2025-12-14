---
title: Upload User Profiles API
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
The Upload User Profiles API enables you to create or update user profiles in CleverTap. 

For example, you can use this API to update a user profile to include information, such as gender, phone, and customer type.

Adding more information to user profiles in CleverTap enables you to create richer segments and run targeted campaigns.  

# Overview
Uploading user profiles to CleverTap requires a POST request with a JSON payload specifying the user profile information. 100 user profiles records can be sent per API call. 

## Base URL
https://api.clevertap.com/1/upload

## HTTP Method
POST

## Headers
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
    "2-1": "Request content-type is always set to application/json; charset=utf-8.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json; charset=utf-8\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Body Parameters
User profiles are uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the user profile.

User profile attribute keys are limited to 120 bytes and their values are limited to 512 bytes. The maximum number of custom user profile keys is 256.

For each user profile, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found a new user profile will be created. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "type",
    "1-0": "profileData",
    "0-1": "Set to profile. Required.",
    "1-1": "User profile properties. Passed as key/value pairs. Required.\nprofileData[“Phone”] should be formatted as +[country code][phone number].",
    "0-2": "string",
    "1-2": "object",
    "0-3": "“profile”",
    "1-3": "\"profileData\": {\n        \"Name\": \"Jack Montana\",\n        \"Email\": \"jack@gmail.com\",\n        \"Phone\": \"+14155551234\",\n        \"Gender\": \"M\",\n        \"Employed\": \"Y\",\n        \"Education\": \"Graduate\",\n        \"Married\": \"Y\",\n        \"MSG-sms\": false,\n        \"DOB\": \"$D_1487576752\",    // UNIX epoch timestamp with $D_ prefix for recording date properties (string)\n        \"Customer Type\": \"Platinum\"\n  }",
    "2-0": "ts",
    "3-0": "identity",
    "2-1": "Date and time when the event occured, Formatted as a UNIX epoch. Defaults to the current timestamp if omitted. Optional.",
    "2-2": "ts",
    "2-3": "1468308340",
    "3-1": "Identity to recognize a unique user. It can be the user’s email, a phone number, or any other identifier that you are using to tag your users. \nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "3-2": "string",
    "3-3": "“jack@gmail.com”",
    "4-0": "FBID",
    "5-0": "GPID",
    "6-0": "objectId",
    "6-1": "Identify a unique user by CleverTap Global Object ID.\nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "5-1": "Identify a unique user by Google Plus ID. \nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "4-1": "Identify a unique user by Facebook ID. \nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "4-3": "“34322423\"",
    "5-3": "“34322424\"",
    "6-3": "“34322425\"",
    "4-2": "string",
    "5-2": "string",
    "6-2": "string"
  },
  "cols": 4,
  "rows": 7
}
[/block]
Here is an example JSON payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"d\": [\n    {\n      \"identity\": \"1189549\",  // customer ID, email or any other ID that uniquely identifies your user\n      \"ts\": 1468308340,       // user creation date, or just leave this field out to set the time has current\n      \"type\": \"profile\",\n      \"profileData\": {\n        \"Name\": \"Jack Montana\",\n        \"Email\": \"jack@gmail.com\",\n        \"Phone\": \"+14155551234\",\n        \"Gender\": \"M\",\n        \"Employed\": \"Y\",\n        \"Education\": \"Graduate\",\n        \"Married\": \"Y\",\n        \"MSG-sms\": false,\n        \"DOB\": \"$D_1487576752\",    // UNIX epoch timestamp with $D_ prefix for recording date properties (string)\n        \"Customer Type\": \"Platinum\"\n      }\n    }\n  ]\n}",
      "language": "json"
    }
  ]
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"d\":[{\"objectId\":\"25b08803c1af4e00839f530264dac6f8\",\"type\":\"profile\",\"profileData\":{\"Name\": \"Jack Test\"}}]}' \"https://api.clevertap.com/1/upload\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json; charset=utf-8\"",
      "language": "curl"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"processed\": 1,\n    \"unprocessed\": []\n}",
      "language": "json"
    }
  ]
}
[/block]
## Debugging

Requests with processing errors will be returned in the API call response as shown in the example below.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\":<success, partial, fail>, \n    \"processed\":<count>, \n    \"unprocessed\": [{\"status\":\"fail\", \"code\":<error code>, \"error\":<error msg>, \"record\":<record>}]\n}",
      "language": "json"
    }
  ]
}
[/block]
To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.

A list of errors code for this API can be found on [this page](doc:api-errors).

## Notes
  * Calls to this endpoint are synchronous. We recommend making individual calls, waiting for our response, and then make another request.
  * Concurrent requests are limited to 3 per account. Requests that exceed the limit will return a 429 HTTP response code.
 
# Next Steps
Now that you’ve learned how to upload user profiles with the CleverTap API, learn how to use the [Get Profile Count API](doc:get-profile-count-api) that lets you get a count of profiles that match a specific query.
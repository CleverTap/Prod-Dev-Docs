---
title: Top Property Counts API
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
# Overview

This endpoint is used to retrieve counts for the most and least frequently occurring properties for a particular event in a specified duration. 

For example, let’s you have an ecommerce app and you are tracking purchase events and storing the product purchased as a property on the event. You can use this endpoint to find out what products are the most frequently purchased. 

## Base URL
https://api.clevertap.com/1/counts/top.json

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
## Body Parameters
The body is uploaded as a JSON payload. 

[block:parameters]
{
  "data": {
    "0-0": "event_name",
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "1-0": "from",
    "2-0": "to",
    "0-1": "The name of the event.",
    "1-1": "Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "2-1": "End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "0-2": "required",
    "1-2": "required",
    "2-2": "required",
    "0-3": "string",
    "1-3": "int",
    "2-3": "int",
    "0-4": "\"choseNewFavoriteFood\"",
    "2-4": "20151025",
    "1-4": "20150810",
    "3-0": "groups",
    "3-1": "Object containing information about properties for which breakdown is required. The endpoint can be used to obtain analysis of multiple properties in one request. Each property object is referenced by a unique key within the groups object.",
    "3-2": "required",
    "3-3": "object",
    "3-4": "\"groups\": {\n\t\t\"foo\": {\n\t\t\t\"property_type\": \"event_properties\",\n\t\t\t\"name\": \"Amount\"\n\t\t},\n\t\t\"bar\": {\n\t\t\t\"property_type\": \"profile_fields\",\n\t\t\t\"name\": \"Customer Type\",\n\t\t\t\"top_n\": 2,\n\t\t\t\"order\": \"asc\"\n\t\t}\n\t}",
    "4-0": "groups.property_type",
    "5-0": "groups.name",
    "6-0": "top_n",
    "7-0": "order",
    "7-1": "Sort order. Either desc or asc. Defaults to desc if absent.",
    "6-1": "Number of top values required for a given property. Can be specified along with property_type and name. Defaults to 10 if absent.",
    "5-1": "Name of the property for which top breakdown is required. Must be present along with property_type.",
    "4-1": "Type of property for which top breakdown is required. Can be event_properties, session_properties, profile_fields, app_fields, demographics, technographics, reachability or geo_fields. Must be present inside each individual group.",
    "4-2": "required",
    "4-3": "string",
    "4-4": "\"event_properties\"",
    "5-2": "required",
    "5-3": "string",
    "5-4": "“Amount”",
    "6-2": "optional",
    "7-2": "optional",
    "6-3": "int",
    "7-3": "string",
    "6-4": "2",
    "7-4": "\"asc\""
  },
  "cols": 5,
  "rows": 8
}
[/block]
The property_type parameter enables you to specify the type of property you need metrics for. Here are the descriptions for the potential parameter values. 
[block:parameters]
{
  "data": {
    "h-0": "property_type",
    "h-1": "Description",
    "0-0": "event_properties",
    "1-0": "profile_fields",
    "2-0": "session_properties",
    "3-0": "app_fields",
    "4-0": "demographics",
    "5-0": "technographics",
    "6-0": "reachability",
    "7-0": "geo_fields",
    "7-1": "Country, region, city.",
    "6-1": "All the reachability fields.",
    "5-1": "All the technographics fields.",
    "4-1": "All the demographics fields.",
    "3-1": "All the app fields.",
    "2-1": "utm_source, utm_medium, utm_campaign, session_referrer, session_source, time_of_day (granularity up to hour of day).",
    "1-1": "All custom profile fields for the account.",
    "0-1": "All event properties for the specified event_name."
  },
  "cols": 2,
  "rows": 8
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"event_name\": \"Charged\",\n\t\"from\": 20161229,\n\t\"to\": 20170129,\n\t\"groups\": {\n\t\t\"foo\": {\n\t\t\t\"property_type\": \"event_properties\",\n\t\t\t\"name\": \"Amount\"\n\t\t},\n\t\t\"bar\": {\n\t\t\t\"property_type\": \"profile_fields\",\n\t\t\t\"name\": \"Customer Type\",\n\t\t\t\"top_n\": 2,\n\t\t\t\"order\": \"asc\"\n\t\t}\n\t}\n}",
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
      "code": "curl -X POST -d '{\"event_name\":\"Charged\",\"from\":20161229,\"to\":20170129,\"groups\":{\"foo\":{\"property_type\":\"event_properties\",\"name\":\"Amount\"},\"bar\":{\"property_type\":\"profile_fields\",\"name\":\"CustomerType\",\"top_n\":2,\"order\":\"asc\"}}}' \"https://api.clevertap.com/1/counts/top.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n  \"status\": \"success\",\n  \"foo\": {\n    \"NUMBER\": {\n      \"0-100\": 10,\n      \"100-200\": 9,\n      \"200-300\": 8,\n      \"300-400\": 7,\n      \"400-500\": 6,\n      \"500-600\": 5,\n      \"600-700\": 4,\n      \"700-800\": 3,\n      \"800-900\": 2,\n      \"900-1000\": 1\n    }\n  },\n  \"bar\": {\n    \"STR\": {\n      \"Gold\": 5,\n      \"Silver\": 10\n    }\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
Each property breakdown is referenced by the unique key assigned to it while querying. Within each property breakdown, there are separate objects specifying the data type for the property’s values that have been provided.

Data types can be STR, NUMBER, ENUM, or DATE. NUMBER and DATE breakdowns are ‘-‘ separated ranges. DATE values are in UNIX epoch format. Counts are present within the data type object for each property breakdown.

## Notes
The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

Here is an example response for a partial status.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"req_id\": 384649162721759,\n  \"status\": \"partial\"\n}",
      "language": "json"
    }
  ]
}
[/block]
After getting the req_id, you will poll the endpoint below and provide the value of req_id as a query parameter.

GET https://api.clevertap.com/1/counts/top.json?req_id=<your_request_id_here>
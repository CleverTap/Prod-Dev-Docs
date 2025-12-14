---
title: CleverTap Query Language
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
The CleverTap Query Language (CQL) can be used to analyze all your event data, identify groups of users, and also target them using our APIs. It can be used with:
  * The [Get User Profiles API](doc:get-user-profiles-api) and [Get Events API](doc:get-events-api) to obtain raw data.
  * The [Report API Endpoints](doc:report-object) to obtain dashboard counts for events, profiles, properties, and trends.
  * The [Create Campaign API](doc:create-campaign-api) to target these users through push and email messages.

# Query Language Template
CQL is expressed as a JSON formatted payload in the API request body.

The base query should generally have the following: 
* an event_name
* from and to dates formatted as integers in YYYYMMDD format
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"event_name\": \"App Launched\",\n    \"from\": 20150810,\n    \"to\": 20151025\n}",
      "language": "json"
    }
  ]
}
[/block]
Additional filters are possible on top of the base query; these include:
  * Event property filters
  * Session property filters
  * Common profile property filters
  * Advanced event filters

Event and session property based filters are applied directly to the base query.

When specifying common profile property or advanced event based filters, the base query can be omitted for target creation & people export APIs and defaults to all events over the past 365 days.

Base event query is mandatory for the event export API in all cases.

# Query Operators & Values
CQL supports (key, operator, value) combinations to filter results.
  * All value fields must be primitives (integer, double, or string), or array(s) of primitives
  * Boolean fields must be passed as a string, with the value “True” or “False”.
  * When the value is a String, the operator can be either `equals`, `not_equals`, `contains`, `does_not_contain`, `exists`, or `does_not_exist`''.
  * When the value is Numeric, the operator can be one of `equals`, `greater_than`, `greater_than_equals`, `less_than`, or `less_than_equals`

While using the items property of the Charged event, which is a list of values, the operator is applied as an OR on all values.

# Event Property Filters
Event property filters enable you to query for User Profiles or Events based on custom event properties.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"event_name\": \"choseNewFavoriteFood\",\n    \"event_properties\": [\n        {\n            \"name\": \"value\",\n            \"operator\": \"contains\",\n            \"value\": \"pizza\"\n        }\n    ],\n    \"from\": 20150810,\n    \"to\": 20151025\n}",
      "language": "json"
    }
  ]
}
[/block]
# Session Property Filters
Session property filters enable you to query for User Profiles or User Events that exhibit certain kind of session information. The following session property filters are currently supported:

All the parameters mentioned below are optional. CQL supports querying on any combinations of these parameters.
[block:parameters]
{
  "data": {
    "0-0": "utm_source",
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Description",
    "1-0": "utm_medium",
    "2-0": "utm_campaign",
    "3-0": "session_referrer",
    "4-0": "session_source",
    "5-0": "first_time",
    "7-0": "time_of_day",
    "8-0": "day_of_month",
    "9-0": "day_of_week",
    "9-1": "string",
    "8-1": "int",
    "7-1": "array of strings (size of 2)",
    "7-2": "Time of day of the event, containing the start and end time range in 24-hour “HH:MM” format.",
    "8-2": "Day of month when the event was raised. The value field can be an integer between 1 and 31 or an array of possible integer values.",
    "9-2": "Day of week when the event was raised. The value field can be a string denoting the day of the week (Sunday, Monday,… Saturday) or an array of possible string values.",
    "5-2": "Filters whether the user performed the event for the first time. The accepted value is “True” or left blank.",
    "5-1": "string",
    "4-1": "string",
    "3-1": "string",
    "2-1": "string",
    "1-1": "string",
    "0-1": "string",
    "0-2": "Value should be a known session source.",
    "1-2": "Value should be a known session medium.",
    "2-2": "Value should be a known session campaign.",
    "3-2": "Value should be a known session referrer.",
    "4-2": "Value should be a known session source. The possible values are Direct, Others, Search, Social, Email, UnAvailable, and Batch.",
    "6-0": "last_time",
    "6-1": "string",
    "6-2": "Filters whether the user performed the event last in the specified time frame. The accepted value is \"True\"."
  },
  "cols": 3,
  "rows": 10
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Session property filters are not supported with the Notification Sent event.",
  "title": "Note"
}
[/block]

**Example**
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"event_name\": \"choseNewFavoriteFood\",\n    \"session_properties\": [\n        {\n            \"name\": \"first_time\",\n            \"value\": \"True\"\n        },\n        {\n            \"name\": \"time_of_day\",\n            \"value\": [\n                \"22:30\",\n                \"22:35\"\n            ]\n        }\n    ],\n    \"from\": 20150810,\n    \"to\": 20151025\n}",
      "language": "json"
    }
  ]
}
[/block]
# Common Profile Properties
Common property filters enable developers to query for people or events that map with certain profile-specific information. The following common profile property filters are currently supported:
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "All the parameters mentioned below are optional. We support querying on any combinations of these parameters."
}
[/block]

**app_fields**
Supports filtering on Operating System (OS) Version, Version (refers to app version), SDK Version, Make and Model of device.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"app_fields\": [\n            {\n                \"name\": \"Make\",\n                \"operator\": \"contains\",\n                \"value\": \"Apple\"\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**profile_fields**
Supports querying custom profile fields.

Supports System Properties:
  * Email: Possible values are valid email addresses.
  * Phone: Possible values are valid phone numbers.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Profile properties such as Name, Identity, and so on are not supported."
}
[/block]
To download based on email/identity, [refer here](https://developer.clevertap.com/docs/get-user-profiles-api#section-download-user-profiles-by-id). 

[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"profile_fields\": [\n            {\n                \"name\": \"language\",\n                \"operator\": \"equals\",\n                \"value\": \"en\"\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**demographics**
Supports filtering on Gender, Age, Education, and Employed. The possible values are:
  * Gender: Possible values are Unknown, Male and Female
  * Age: Possible values are: 
    * Unknown
    * Kid: age < 18 years
    * Youth: 18-24 age
    * Young_Adult: 25-40 age
    * Adult: 41-60 age
    * Senior: age > 60 years
  * Education: Possible values are Unknown, School, College, and Graduate
  * Employed: Possible values are Unknown, Yes, and No 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"demographics\": [\n            {\n                \"name\": \"Education\",\n                \"operator\": \"equals\",\n                \"value\": [\n                    \"College\",\n                    \"Graduate\"\n                ]\n            },\n            {\n                \"name\": \"Gender\",\n                \"value\": \"Female\"\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**technographics**
Supports filtering on Platform, Platform Type, Device, Operating System(OS), and Browser. The possible values are:
  * Platform: Possible values are WebApp, AndroidApp, iOSApp, and WindowsApp
  * Platform Type: Possible values are WebApp, MobileApp
  * Device: Possible values are Desktop, Mobile, Tablet, and TV
  * Operating System(OS): Possible values are Others, Android, iOS, Windows, Mac, BlackBerry, and Linux
  * Browser – Possible values are Others, Firefox, Chrome, Seamonkey, Safari, Opera, IE, and UCBrowser 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"technographics\": [\n            {\n                \"name\": \"Device\",\n                \"operator\": \"equals\",\n                \"value\": [\n                    \"Mobile\",\n                    \"Tablet\"\n                ]\n            },\n            {\n                \"name\": \"Platform\",\n                \"value\": \"AndroidApp\"\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**reachability**
Supports filtering on the following:
  * has_token: Determines whether the user has a device token
  * has_email: Determines whether the user has an email address associated
  * has_phone_number: Determines when the user has a phone number associated
  * MSG-push: False indicates that the user has unsubscribed from push notifications
  * MSG-email: False indicates that the user has unsubscribed from email notifications
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"reachability\": [\n            {\n                \"name\": \"has_token\",\n                \"value\": \"True\"\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**geo_fields**
Supports filtering on geolocation information. It must be formatted as one or more Array(s) of Strings, each of size 3, in [Country, Region, City] format.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"geo_fields\": [\n            [\n                \"India\",\n                \"Maharashtra\",\n                \"Mumbai\"\n            ],\n            [\n                \"United States\",\n                \"New York\",\n                \"New York City\"\n            ]\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**geo_radius**
Supports filtering on geo radius. Must be formatted as one or more Array(s) of numbers (Int/Double), each of size 3, in [Latitude, Longitude, Radius] format

Radius is expressed in kilometers
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n        \"geo_radius\": [\n            [\n                35,\n                42.5,\n                5.2\n            ],\n            [\n                77.6,\n                76.2,\n                11\n            ]\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
# Advanced Queries

**Query With Did None Block**
To segment users who performed NONE of the specified action(s) within the given range.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"advanced_query\": {\n        \"did_none\": [\n            {\n                \"event_name\": \"Charged\",\n                \"from\": 20151010,\n                \"to\": 20151011,\n                \"event_properties\": [\n                    {\n                        \"name\": \"Amount\",\n                        \"operator\": \"equals\",\n                        \"value\": 100\n                    }\n                ]\n            },\n            {\n                \"event_name\": \"App Launched\",\n                \"from\": 20151011,\n                \"to\": 20151012\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
**Query With Did All Block**
To segment users who performed ALL of the specified action(s) within the given range.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"advanced_query\": {\n        \"did_all\": [\n            {\n                \"event_name\": \"Charged\",\n                \"from\": 20151010,\n                \"to\": 20151011,\n                \"event_properties\": [\n                    {\n                        \"name\": \"Feedback\",\n                        \"operator\": \"contains\",\n                        \"value\": \"excellent\"\n                    }\n                ]\n            },\n            {\n                \"event_name\": \"App Launched\",\n                \"from\": 20151011,\n                \"to\": 20151012,\n                \"operator\": \"equals\",\n                \"value\": 10\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The operator and value are optional within each internal event block. The default values for the same are “greater_than_equals” and 1, respectively."
}
[/block]
**Query With Did Any Block**
To segment users who performed ANY of the specified action(s) within the given range.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"advanced_query\": {\n        \"did_any\": {\n            \"any_events\": [\n                {\n                    \"event_name\": \"Charged\",\n                    \"from\": 20151010,\n                    \"to\": 20151011,\n                    \"event_properties\": [\n                        {\n                            \"name\": \"Amount\",\n                            \"operator\": \"equals\",\n                            \"value\": 100\n                        }\n                    ]\n                },\n                {\n                    \"event_name\": \"App Launched\",\n                    \"from\": 20151011,\n                    \"to\": 20151012\n                }\n            ],\n            \"operator\": \"equals\",\n            \"value\": 10\n        }\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The operator and value are for the entire “any_events” block and resolved for the entire combination of events. They are optional, with default values being “greater_than_equals” and 1, respectively."
}
[/block]
# Sample Queries 
We provided code snippets on GitHub showing some sample queries. Please see the linked examples below:
  * [Basic event query](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/basic_event_query_request_payload.json)
  * [Query with event/session properties](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_event_session_properties_request_payload.json)
  * [Query with common profile properties](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_common_properties_request_payload.json)
  * [Query with did none block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_none_block_request_payload.json)
  * [Query with did all block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_all_block_request_payload.json)
  * [Query with did any block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_any_block_request_payload.json)
  * [Sample hybrid query](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/sample_hybrid_query_request_payload.json)

**Fetch profiles based on Subscription status**
Supports fetching profiles based on the subscription status of any group for the account
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"common_profile_properties\": {\n           \"subscription_group\": [\n            {\n                \"name\": \"Email\",\n                \"operator\": \"Included\",//can use Excluded too\n                \"value\": [\n                \t\"Group1\"\n                ]\n            }\n        ]\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
# Special Events

## Notification Events
Notification events are special system events that provide information about users receiving notifications through CleverTap. They support filtering by the following event properties:

* Campaign ID (Integer ID of the campaign shown on the dashboard)
* Campaign Type can have the following possible values (passed as a string or array of strings):
  * Mobile Push - Android 
  * Mobile Push - iOS 
  * Mobile Push - Windows 
  * Email 
  * Facebook Audiences
  * SMS 
  * Web Push - Chrome

Notification events are available for filtering on the [Get User Profiles API](doc:get-user-profiles-api) and the [Create Campaign API](doc:create-campaign-api), as part of the where clause.
[block:callout]
{
  "type": "info",
  "body": "Filtering by session properties is not supported for the Notification events as they are system events.",
  "title": "Filter by Session Property"
}
[/block]
The following is the list of Notification Events:
* Notification Sent
* Notification Replied
* Notification Delivered
* Notification Clicked
* Notification Viewed
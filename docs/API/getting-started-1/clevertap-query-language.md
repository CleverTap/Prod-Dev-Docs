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
* The [Report API Endpoints](doc:report-object) to obtain dashboard counts for events, profiles, properties and trends.
* The [Create Campaign API](doc:create-campaign-api) to target these users through push and email messages.

# Query Language Template

CQL is expressed as a JSON formatted payload in the API request body.

In general, the base query should have an event\_name, and from & to dates formatted as integers in YYYYMMDD format.

```json
{
    "event_name": "App Launched",
    "from": 20150810,
    "to": 20151025
}
```

Additional filters are possible on top of the base query, these include:

* Event property filters
* Session property filters
* Common profile property filters
* Advanced event filters

Event and session property based filters are applied directly to the base query.

When specifying common profile property or advanced event based filters, the base query can be omitted for target creation & people export APIs, and defaults to all events over the past 365 days.

Base event query is mandatory for the event export API in all cases.

# Query Operators & Values

CQL supports (key, operator, value) combinations to filter results.

* All value fields must be primitives (integer, double, or string), or array(s) of primitives
* Boolean fields must be passed as string, with value “True” or “False”.
* When the value is a String, the operator can be either `equals`, `not_equals`,`contains`,`does_not_contain`,`exists `,`does_not_exist`''.
* When the value is Numeric, the operator can be one of `equals`, `greater_than`, `greater_than_equals`, `less_than` or `less_than_equals`

While using the items property of the Charged event, which is a list of values, the operator is applied as an OR on all values.

# Event Property Filters

Event property filters enable you to query for User Profiles or Events based on custom event properties.

```json
{
    "event_name": "choseNewFavoriteFood",
    "event_properties": [
        {
            "name": "value",
            "operator": "contains",
            "value": "pizza"
        }
    ],
    "from": 20150810,
    "to": 20151025
}
```

# Session Property Filters

Session property filters enable you to query for User Profiles or User Events that exhibit certain kind of session information. The following session property filters are currently supported:

All the parameters mentioned below are optional. CQL supports querying on any combinations of these parameters.

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Key
      </th>

      <th>
        Type
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        utm\_source
      </td>

      <td>
        string
      </td>

      <td>
        Value should be a known session source.
      </td>
    </tr>

    <tr>
      <td>
        utm\_medium
      </td>

      <td>
        string
      </td>

      <td>
        Value should be a known session medium.
      </td>
    </tr>

    <tr>
      <td>
        utm\_campaign
      </td>

      <td>
        string
      </td>

      <td>
        Value should be a known session campaign.
      </td>
    </tr>

    <tr>
      <td>
        session\_referrer
      </td>

      <td>
        string
      </td>

      <td>
        Value should be a known session referrer.
      </td>
    </tr>

    <tr>
      <td>
        session\_source
      </td>

      <td>
        string
      </td>

      <td>
        Value should be a known session source. Possible values are Direct, Others, Search, Social, Email, UnAvailable, Batch.
      </td>
    </tr>

    <tr>
      <td>
        first\_time
      </td>

      <td>
        string
      </td>

      <td>
        Filters whether the user performed the event for the first time. Accepted value is “True”, or left blank.
      </td>
    </tr>

    <tr>
      <td>
        time\_of\_day
      </td>

      <td>
        array of strings (size of 2)
      </td>

      <td>
        Time of day of the event, containing the start and end time range in 24 hour “HH:MM” format.
      </td>
    </tr>

    <tr>
      <td>
        day\_of\_month
      </td>

      <td>
        int
      </td>

      <td>
        Day of month when the event was raised. Value field can be an integer between 1 and 31, or an array of possible integer values.
      </td>
    </tr>

    <tr>
      <td>
        day\_of\_week
      </td>

      <td>
        string
      </td>

      <td>
        Day of week when the event was raised. Value field can be a string denoting day of week (Sunday, Monday,… Saturday), or an array of possible string values.
      </td>
    </tr>
  </tbody>
</Table>

Session property filters are not supported with Notification Sent event.

**Example**

```json
{
    "event_name": "choseNewFavoriteFood",
    "session_properties": [
        {
            "name": "first_time",
            "value": "True"
        },
        {
            "name": "time_of_day",
            "value": [
                "22:30",
                "22:35"
            ]
        }
    ],
    "from": 20150810,
    "to": 20151025
}
```

# Common Profile Properties

Common property filters enable developers to query for people or events that map with certain profile specific information. The following common profile property filters are currently supported:

All the parameters mentioned below are optional. We support querying on any combinations of these parameters.

**app\_fields**\
Supports filtering on OS Version, Version (refers to app version), SDK Version, Make and Model of device.

```json
{
    "common_profile_properties": {
        "app_fields": [
            {
                "name": "Make",
                "operator": "contains",
                "value": "Apple"
            }
        ]
    }
}
```

**profile\_fields**\
Supports querying custom profile fields.

Supports System Properties:

* Email – Possible values are valid email addresses.
* Phone – Possible values are valid phone numbers.

Profile properties such as Name, Identity, and so on are not supported. To download based on email/identity, refer here. 

```json
{
    "common_profile_properties": {
        "profile_fields": [
            {
                "name": "language",
                "operator": "equals",
                "value": "en"
            }
        ]
    }
}
```

**demographics**\
Supports filtering on Gender, Age, Education, and Employed. Possible values are:

* Gender – Possible values are Unknown, Male and Female
* Age – Possible values are Unknown, Kid, Youth, Young\_Adult, Adult and Senior where Kid refers to age \< 18 years, Youth is 18-24, Young\_Adult is 25-40, Adult is 41-60 and Senior is > 60 years
* Education – Possible values are Unknown, School, College, and Graduate
* Employed – Possible values are Unknown, Yes and No 

```json
{
    "common_profile_properties": {
        "demographics": [
            {
                "name": "Education",
                "operator": "equals",
                "value": [
                    "College",
                    "Graduate"
                ]
            },
            {
                "name": "Gender",
                "value": "Female"
            }
        ]
    }
}
```

**technographics**\
Supports filtering on Platform, Platform Type, Device, OS and Browser. Possible values are:

* Platform – Possible values are WebApp, AndroidApp, iOSApp and WindowsApp
* Platform Type – Possible values are WebApp, MobileApp
* Device – Possible values are Desktop, Mobile, Tablet and TV
* OS – Possible values are Others, Android, iOS, Windows, Mac, BlackBerry and Linux
* Browser – Possible values are Others, Firefox, Chrome, Seamonkey, Safari, Opera, IE and UCBrowser 

```json
{
    "common_profile_properties": {
        "technographics": [
            {
                "name": "Device",
                "operator": "equals",
                "value": [
                    "Mobile",
                    "Tablet"
                ]
            },
            {
                "name": "Platform",
                "value": "AndroidApp"
            }
        ]
    }
}
```

**reachability**\
Support filtering on the following:

* has\_token (whether the user has a device token)
* has\_email (whether the user has an email address associated)
* has\_phone\_number (when the user has a phone number associated)
* MSG-push (False indicates that the user has unsubscribed from push notifications)
* MSG-email (False indicates that the user has unsubscribed from email notifications) 

```json
{
    "common_profile_properties": {
        "reachability": [
            {
                "name": "has_token",
                "value": "True"
            }
        ]
    }
}
```

**geo\_fields**\
Supports filtering on geo location info. Must be formatted as one or more Array(s) of Strings, each of size 3, in [Country, Region, City] format.

```json
{
    "common_profile_properties": {
        "geo_fields": [
            [
                "India",
                "Maharashtra",
                "Mumbai"
            ],
            [
                "United States",
                "New York",
                "New York City"
            ]
        ]
    }
}
```

**geo\_radius**\
Supports filtering on geo radius. Must be formatted as one or more Array(s) of numbers (Int/Double), each of size 3, in [Latitude, Longitude, Radius] format

Radius is expressed in kilometers

```json
{
    "common_profile_properties": {
        "geo_radius": [
            [
                35,
                42.5,
                5.2
            ],
            [
                77.6,
                76.2,
                11
            ]
        ]
    }
}
```

# Advanced Queries

**Query With Did None Block**\
To segment users who performed NONE of the specified action(s) within the given range.

```json
{
    "advanced_query": {
        "did_none": [
            {
                "event_name": "Charged",
                "from": 20151010,
                "to": 20151011,
                "event_properties": [
                    {
                        "name": "Amount",
                        "operator": "equals",
                        "value": 100
                    }
                ]
            },
            {
                "event_name": "App Launched",
                "from": 20151011,
                "to": 20151012
            }
        ]
    }
}
```

**Query With Did All Block**\
To segment users who performed ALL of the specified action(s) within the given range.

```json
{
    "advanced_query": {
        "did_all": [
            {
                "event_name": "Charged",
                "from": 20151010,
                "to": 20151011,
                "event_properties": [
                    {
                        "name": "Feedback",
                        "operator": "contains",
                        "value": "excellent"
                    }
                ]
            },
            {
                "event_name": "App Launched",
                "from": 20151011,
                "to": 20151012,
                "operator": "equals",
                "value": 10
            }
        ]
    }
}
```

Note: The operator and value are optional within each internal event block. Default values for the same are “greater\_than\_equals” and 1 respectively.

**Query With Did Any Block**\
To segment users who performed ANY of the specified action(s) within the given range.

```json
{
    "advanced_query": {
        "did_any": {
            "any_events": [
                {
                    "event_name": "Charged",
                    "from": 20151010,
                    "to": 20151011,
                    "event_properties": [
                        {
                            "name": "Amount",
                            "operator": "equals",
                            "value": 100
                        }
                    ]
                },
                {
                    "event_name": "App Launched",
                    "from": 20151011,
                    "to": 20151012
                }
            ],
            "operator": "equals",
            "value": 10
        }
    }
}
```

Note: The operator and value are for the entire “any\_events” block, and are resolved for the entire combination of events present within. They are optional, with default values being “greater\_than\_equals” and 1 respectively.

# Sample Queries

We provided code snippets on GitHub showing some sample queries. Please see the linked examples below:

* [Basic event query](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/basic_event_query_request_payload.json)
* [Query with event/session properties](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_event_session_properties_request_payload.json)
* [Query with common profile properties](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_common_properties_request_payload.json)
* [Query with did none block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_none_block_request_payload.json)
* [Query with did all block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_all_block_request_payload.json)
* [Query with did any block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_any_block_request_payload.json)
* [Sample hybrid query](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/sample_hybrid_query_request_payload.json)

**Fetch profiles based on Subscription status**\
Supports fetching profiles based on the subscription status of any group for the account

```json
{
    "common_profile_properties": {
           "subscription_group": [
            {
                "name": "Email",
                "operator": "Included",//can use Excluded too
                "value": [
                	"Group1"
                ]
            }
        ]
    }
}
```

# Special Events

**Notification Sent**\
Notification Sent is a special system event that can be used to obtain information about users receiving notifications through CleverTap. It supports filtering by the event properties Campaign ID (integer ID of the campaign shown on the dashboard) and Campaign Type.

Campaign Type can have the following possible values (passed as string or array of string) – "Mobile Push - Android", "Mobile Push - iOS", "Mobile Push - Windows", "Email", "Facebook Audiences", "SMS", "Web Push - Chrome".

Notification Sent event is available for filtering on the [Get User Profiles API](doc:get-user-profiles-api)  and the [Create Campaign API](doc:create-campaign-api), as part of the where clause.

Filtering by session properties is not supported for the Notification Sent event as it is a system event.

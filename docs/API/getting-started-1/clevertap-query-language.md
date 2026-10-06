---
title: CleverTap Query Language
excerpt: Understand the usage of CleverTap Query Language (CQL).
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The CleverTap Query Language (CQL) can be used to analyze all your event data, identify user groups, and target them using our APIs.

CQL can be used with:

* The [Get User Profiles](doc:get-user-profiles-api), [Get Events](doc:get-events-api), and [Get Profile Count APIs](doc:get-profile-count-api) to obtain raw data.
* The [Report APIs](doc:report-object) to obtain dashboard counts for events, profiles, properties, and trends.
* The [Create Campaign API](doc:create-campaign-api) to target these users with push and email messages.

# Query Language Template

CQL is expressed as a JSON-formatted payload in the API request body.

The base query should generally have the following:

* An `event_name`
* `from` and `to` dates are formatted as integers in **YYYYMMDD** format.

```json
{
    "event_name": "App Launched",
    "from": 20150810,
    "to": 20151025
}
```

Additional filters are possible on top of the base query; these include:

* Event property filters
* Session property filters
* Common profile property filters
* Advanced event filters

Event and session property-based filters are applied directly to the base query.

When specifying common profile properties or advanced event-based filters, the base query can be omitted for target creation & people export APIs, and defaults to all events over the past 365 days.

Base event query is mandatory for the event export API in all cases.

# Query Operators & Values

CQL supports (key, operator, value) combinations to filter results.

* All value fields must be primitives (integer, double, or string), or array(s) of primitives.
* Boolean fields must be passed as a string, with the value “True” or “False”.
* When the value is a String, the operator can be either `equals`, `not_equals`, `contains`, `does_not_contain`, `exists`, or `does_not_exist`''.
* When the value is Numeric, the operator can be one of `equals`, `greater_than`, `greater_than_equals`, `less_than`, or `less_than_equals`

When using the items property of the Charged event, which is a list of values, the operator is applied to one OR all of the values.

# Event Property Filters

Event property filters enable you to query for User Profiles or Events based on custom event properties as show below:

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

Session property filters enable you to query for User Profiles or User Events that exhibit certain kinds of session information. The following session property filters are currently supported:

All the parameters mentioned below are optional. CQL supports querying on any combination of these parameters.

| Key              | Type                         | Description                                                                                                                                                            |
| :--------------- | :--------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| utm_source       | string                       | Value should be a known session source.                                                                                                                                |
| utm_medium       | string                       | Value should be a known session medium.                                                                                                                                |
| utm_campaign     | string                       | Value should be a known session campaign.                                                                                                                              |
| session_referrer | string                       | Value should be a known session referrer.                                                                                                                              |
| session_source   | string                       | Value should be a known session source. The possible values are Direct, Others, Search, Social, Email, UnAvailable, and Batch.                                         |
| first_time       | string                       | Filters whether the user performed the event for the first time. The accepted value is “True” or left blank.                                                           |
| last_time        | string                       | Filters whether the user performed the event last in the specified time frame. The accepted value is "True".                                                           |
| time_of_day      | array of strings (size of 2) | Time of day of the event, containing the start and end time range in 24-hour “HH:MM” format.                                                                           |
| day_of_month     | int                          | Day of the month when the event was raised. The value field can be an integer between 1 and 31 or an array of possible integer values.                                 |
| day_of_week      | string                       | Day of week when the event was raised. The value field can be a string denoting the day of the week (Sunday, Monday,… Saturday) or an array of possible string values. |

<Callout icon="📘" theme="info">
  #### Note

  Session property filters are not supported with the **Notification Sent** event.
</Callout>

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

# Common Property Filters

Common property filters enable developers to query for people or events that match certain profile-specific information. The following common profile property filters are currently supported:

<Callout icon="📘" theme="info">
  #### Note

  All the parameters mentioned below are optional. We support querying on any combinations of these parameters.
</Callout>

### app_fields

It supports filtering on Operating System (OS) Version, Version (refers to app version), SDK Version, Make, and Model of device, as shown below:

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

### profile_fields

* It supports querying custom profile fields.
* It supports the following System Properties: Email and Phone

<Callout icon="📘" theme="info">
  ### Note

  Profile properties such as Name and Identity are not supported.
</Callout>

To download based on email/identity, refer [here](https://developer.clevertap.com/docs/get-user-profiles-api#section-download-user-profiles-by-id).

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

<Callout icon="🚧" theme="warn">
  #### Empty values in  `profile_fields`

  Avoid using empty or blank values (`""`) as targeting values in a `profile_fields` condition. Always set a specific value that matches who you intend to target. This applies across all profile attributes.

  Leaving a value empty does not result into an error, but it changes the caondition for the target audience. The operators behave differently when the value is empty:

  - `not_equals` with `value: ""` matches all users whose field value is anything other than an empty string.
  - `contains` or `starts_with` with `value: ""` matches all users who have that field populated with any values.
  - `equals` with `value: ""` matches all users.

  For example, use a specific value such as `"value": "example@domain.com"`, or  `"operator": "contains", "value": "@domain.com"`, not an empty string.

  This applies to Event properties as well.
</Callout>

### demographics

It supports filtering on Gender, Age, Education, and Employed as shown below:

The possible values are:

* Gender: Possible values are Unknown, Male, and Female.
* Age: Possible values are:
  * Unknown
  * Kid: age \< 18 years
  * Youth: 18-24 age
  * Young_Adult: 25-40 age
  * Adult: 41-60 age
  * Senior: age > 60 years
* Education: Possible values are Unknown, School, College, and Graduate
* Employed: Possible values are Unknown, Yes, and No

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

### technographics

It supports filtering on Platform, Platform Type, Device, OS (Operating System), and Browser as shown below:

The possible values are:

* Platform: Possible values are WebApp, AndroidApp, iOSApp, and WindowsApp
* Platform Type: Possible values are WebApp, MobileApp
* Device: Possible values are Desktop, Mobile, Tablet, and TV
* OS: Possible values are Others, Android, iOS, Windows, Mac, BlackBerry, and Linux
* Browser: Possible values are Others, Firefox, Chrome, Seamonkey, Safari, Opera, IE, and UCBrowser

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

### reachability

It supports filtering on the following:

* has_token: Determines whether the user has a device token.
* has_email: Determines whether the user has an email address associated.
* has_phone_number: Determines when the user has a phone number associated.
* MSG-push: False indicates that the user has unsubscribed from push notifications.
* MSG-email: False indicates that the user has unsubscribed from email notifications.

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

### geo_fields

It supports filtering on geolocation information. It must be formatted as one or more Array(s) of Strings, each of size 1, 2, or 3, in \[Country, Region, City] format.

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

### geo_radius

It supports filtering on geo radius. It must be formatted as one or more Array(s) of numbers (Int/Double), each of size 3, in \[Latitude, Longitude, Radius] format.

Radius is expressed in kilometers.

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

### segment_membership

Use this to filter users based on their membership in existing CleverTap dashboard segments.

#### Rules

* Segment names are case-sensitive and must match exactly.
* At least one of `include` or `exclude` must be specified.
* Segment names must be non-empty strings.

#### Example

```json
{
  "common_profile_properties": {
    "segment_membership": {
      "include": ["VIP Users"],
      "exclude": ["Dormant Users"]
    }
  }
}
```

<Callout icon="📘" theme="info">
  #### Note

  This filter is evaluated using exact segment names defined in the dashboard.
</Callout>

### user_bucket

Use this to filter users based on their assigned bucket values. You can specify bucket filters using a list of bucket IDs or define a range with an operator. For more information, refer to [User Bucket](https://staging.docs.user.clevertap.net/docs/user-bucket).

#### Supported Formats

* **List of Bucket IDs**

  ```json
  {
    "common_profile_properties": {
      "user_bucket": [1, 2, 3]
    }
  }
  ```

* **Range with Operator**
  ```json
  {
    "common_profile_properties": {
      "user_bucket": {
        "range": { "from": 1, "to": 10 },
        "operator": "equals"
      }
    }
  }
  ```

* **Explicit List with Inversion**
  ```json
  {
    "common_profile_properties": {
      "user_bucket": {
        "list": [1, 2, 3],
        "invert": true
      }
    }
  }
  ```

#### Rules

* Bucket values must be integers.
* When using the range operator, the operator field is mandatory.

# Advanced Queries

### Query With Did None Block

This query is used to segment users who performed NONE of the specified action(s) within the given range, as shown below:

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

### Query With Did All Block

This is used to segment users who performed ALL of the specified action(s) within the given range, as shown below:

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

<Callout icon="📘" theme="info">
  #### Note

  The operator and value are optional within each internal event block. The default values for the same are “greater_than_equals” and 1, respectively.
</Callout>

### Query With Did Any Block

This is used to segment users who performed ANY of the specified action(s) within the given range, as shown below:

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

<Callout icon="📘" theme="info">
  #### Note

  The operator and value are for the entire “any_events” block and resolved for the entire combination of events. They are optional, with default values being “greater_than_equals” and 1, respectively.
</Callout>

# Likelihood (Behavior Analysis)

The `likelihood` parameter enables you to define dominant or significant behavioral patterns for events.

It allows you to evaluate how frequently a user performs an action under specific conditions, such as time of day, day of week, or event property values.

## Usage

You can use `likelihood` in the following places:

* Top-level event query
* Advanced query blocks:
  * `advanced_query.did_all[]`
  * `advanced_query.did_any.any_events[]`
  * `advanced_query.did_none[]`

## Fields

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Field
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
        type
      </td>

      <td>
        String
      </td>

      <td>
        * Defines the behavior type.
        * Possible values:
          * `event_property` : Requires an event property filter object.
          * `time_of_day`:
            * `from_hour`: Integer (0–23)
            * `to_hour`: Integer (0–23)
          * `day_of_week`: Accepts a single day or an array of days (for example, `Monday`, `Tuesday`).
      </td>
    </tr>

    <tr>
      <td>
        match_mode
      </td>

      <td>
        String
      </td>

      <td>
        * Defines how the condition is evaluated.
        * Possible values: `predominant` (default), `at_least`
      </td>
    </tr>

    <tr>
      <td>
        significance_factor
      </td>

      <td>
        Double
      </td>

      <td>
        * Required when `match_mode` is `at_least`.
        * Value must be between 0.0 and 1.0
      </td>
    </tr>
  </tbody>
</Table>

## Example: Predominant Usage (Time of Day)

```json
{
  "event_name": "App Launched",
  "from": 20200101,
  "to": 20200131,
  "likelihood": {
    "type": "time_of_day",
    "match_mode": "predominant",
    "time_of_day": {
      "from_hour": 8,
      "to_hour": 10
    }
  }
}
```

## Example: Minimum Threshold Usage

```json
{
  "event_name": "App Launched",
  "from": 20200101,
  "to": 20200131,
  "likelihood": {
    "type": "time_of_day",
    "match_mode": "at_least",
    "time_of_day": {
      "from_hour": 8,
      "to_hour": 10
    },
    "significance_factor": 0.25
  }
}
```

# Sample Queries

Sample code snippets have been added to the GitHub repository. For more information, refer to the following links

* [Basic event query](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/basic_event_query_request_payload.json)
* [Query with event/session properties](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_event_session_properties_request_payload.json)
* [Query with common profile properties](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_common_properties_request_payload.json)
* [Query with did none block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_none_block_request_payload.json)
* [Query with did all block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_all_block_request_payload.json)
* [Query with did any block](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/query_with_did_any_block_request_payload.json)
* [Sample hybrid query](https://github.com/CleverTap/clevertap-support-snippets/blob/master/clevertap_query_language/examples/sample_hybrid_query_request_payload.json)

### Fetch Profiles Based on Subscription Status

It supports fetching profiles based on the subscription status of any group for the account.

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

### Notification Events

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

The following is the list of Notification Events:

* Notification Sent
* Notification Replied
* Notification Delivered
* Notification Clicked
* Notification Viewed
* These events are available for filtering on the [Get User Profiles API](doc:get-user-profiles-api) and the [Create Campaign API](doc:create-campaign-api), as part of the where clause.

<Callout icon="📘" theme="info">
  #### Filter by Session Property

  Filtering by session properties is not supported for the Notification events as they are system events.
</Callout>

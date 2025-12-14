---
title: Get Message Reports API
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
The Get Message Reports API lets you download a list of messages sent by CleverTap. 

For example, you can this API to get a report of how many in-app messages and push notifications were sent to users in the past week. 

# Overview

To get a message report, you submit a request with the date range needed. You can also filter these results by setting optional parameters that will return only specific channels or message statuses.

## Base URL

[https://api.clevertap.com/1/message/report.json](https://api.clevertap.com/1/message/report.json)

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap Account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap Account Passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/json.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

The body is uploaded as a JSON payload. from & to are required parameters. All the optional parameters, except daily, let input multiple values by separating them with commas.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        from
      </td>

      <td>
        Start of date range for events needed. Value specified in format YYYYMMDD. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “20171201”
      </td>
    </tr>

    <tr>
      <td>
        to
      </td>

      <td>
        End of date range for events needed. Value specified in format YYYYMMDD. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “20171225”
      </td>
    </tr>

    <tr>
      <td>
        channel
      </td>

      <td>
        The channels of communication needed. Supported values include push (mobile push messages), email, sms, browser (browser push messages), audiences (Facebook messages), inapp (in-app messages), webhooks (webhooks), web\_pop\_up (Web pop-ups), web\_exit\_intent (web exit intent pop-ups). Optional.
      </td>

      <td>
        array of strings
      </td>

      <td>
        [“push”,”email”]
      </td>
    </tr>

    <tr>
      <td>
        delivery
      </td>

      <td>
        Delivery type of communications for which you want a report. Supported input values are one\_time, inaction, action, recurring, property\_time, and api. Optional.
      </td>

      <td>
        array of strings
      </td>

      <td>
        [“inaction”,”action”]
      </td>
    </tr>

    <tr>
      <td>
        daily
      </td>

      <td>
        Determines if the report should be a day-wise split, or an aggregate report. Supported values are true or false. Unless specified otherwise, the value of daily is set to false. Optional.
      </td>

      <td>
        boolean
      </td>

      <td>
        false
      </td>
    </tr>

    <tr>
      <td>
        status
      </td>

      <td>
        Determines the status of messages for which you want a report. Supported values include scheduled, running, stopped, and completed. Optional.
      </td>

      <td>
        array of strings
      </td>

      <td>
        [“completed”,”running”]
      </td>
    </tr>

    <tr>
      <td>
        message\_type
      </td>

      <td>
        Determines the type of messages for which you want a report. Supported values include single, ab, message\_on\_user\_property. Optional.
      </td>

      <td>
        array of strings
      </td>

      <td>
        [“single”, “ab”]
      </td>
    </tr>

    <tr>
      <td>
        label
      </td>

      <td>
        Result will include a report only for messages that have the labels specified in your query. Optional.
      </td>

      <td>
        array of strings
      </td>

      <td>
        [“Onboarding”]
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
   "from":"20171011",
   "to":"20171130",
   "daily":false,
   "channel":[
    "InApp"
   ],
   "delivery":[
    "inaction", "action"
   ],
   "status":[
    "completed"
   ],
   "message_type":[
    "single"
   ],
   "label":[
   ]
}
```

## Example Request

```curl
curl -X POST -d '{"from":"20171101","to":"20171225"}' "https://api.clevertap.com/1/message/report.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

## Example Response

```json
{
    "status": "success",
    "total_results": 1,
    "messages": [
        {
            "message id": 1508323121,
            "data": [
                [
                    {
                        "sent": 0,
                        "viewed": 0,
                        "clicked": 0
                    }
                ]
            ],
            "start_date": "Oct 18, 4:08 PM",
            "device": [
                "Android",
                "iOS",
                "WindowsMobile"
            ],
            "conversion_event": null,
            "labels": [],
            "channel": "InApp",
            "message_name": "in_app_outputs_test",
            "delivery": "Action"
        }
    ]
}
```

# Next Steps

Now that you’ve learned how to get a message report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).

---
title: Upload Events API
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
The Upload Events API enables you to upload user events to CleverTap. 

For example, you can use this API to store user events, such as product views and purchases. You associate these events with a specific user by using an identifier, such as an email address or a phone number. 

Storing user events in CleverTap enables you to create richer segments and run targeted campaigns.

# Overview

Uploading events to CleverTap requires a POST request with a JSON payload specifying the event information. 1000 event records can be sent per API call. 

The maximum number of event types per account is 500. For each event type, the maximum number of properties is 256. The number of events submitted per account across those event types is unlimited. 

## Base URL

[https://api.clevertap.com/1/upload](https://api.clevertap.com/1/upload)

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
        Request content-type is always set to application/json; charset=utf-8.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json; charset=utf-8"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

Events are uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the event including the event name, event properties, timestamp, and user identifier.

Event keys are limited to 120 characters and property values are limited to 512 bytes.

For each event, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found a new user profile will be created. 

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
        type
      </td>

      <td>
        Set to event. Required.
      </td>

      <td>
        string
      </td>

      <td>
        "event"
      </td>
    </tr>

    <tr>
      <td>
        evtName
      </td>

      <td>
        Name of event. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “Product Viewed”
      </td>
    </tr>

    <tr>
      <td>
        evtData
      </td>

      <td>
        Event properties describing the event. Passed as key/value pairs. Required.
      </td>

      <td>
        object
      </td>

      <td>
        "evtData":\
        \{ "Product name": "Casio Watch", "Category": "Mens Watch" }
      </td>
    </tr>

    <tr>
      <td>
        ts
      </td>

      <td>
        Date and time when the event occured, Formatted as a UNIX epoch. Defaults to the current timestamp if omitted. Optional.
      </td>

      <td>
        ts
      </td>

      <td>
        1468308340
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Identity to recognize a unique user. It can be the user’s email, a phone number, or any other identifier that you are using to tag your users.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “[jack@gmail.com](mailto:jack@gmail.com)”
      </td>
    </tr>

    <tr>
      <td>
        FBID
      </td>

      <td>
        Identify a unique user by Facebook ID. 

        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322423"
      </td>
    </tr>

    <tr>
      <td>
        GPID
      </td>

      <td>
        Identify a unique user by Google Plus ID. 

        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322424"
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        Identify a unique user by CleverTap Global Object ID.

        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322425"
      </td>
    </tr>
  </tbody>
</Table>

Here is an example JSON payload.

```json
{
    "d": [
        {
            "FBID": "34322423",
            "ts": 1468308340, // time when the event occurred
            "type": "event",
            "evtName": "Product viewed",
            "evtData": {
                "Product name": "Casio Chronograph Watch",
                "Category": "Mens Watch",
                "Price": 59.99,
                "Currency": "USD"
            }
        },
        {
            "identity": "jack@gmail.com",
            "ts": 1468208340,
            "type": "event",
            "evtName": "Charged",
            "evtData": {
                "Amount": 300,
                "Currency": "USD",
                "Payment mode": "Credit Card",
                "Delivery By": "$D_1468322268",     
                "Items": [
                    {
                        "Category": "books",
                        "Book name": "The millionaire next door",
                        "Quantity": 1
                    },
                    {
                        "Category": "books",
                        "Book name": "Achieving inner zen",
                        "Quantity": 4
                    }
                ]
            }
        }
    ]
}
```

## Example Request

```curl
curl -X POST -d '{ "d": [ { "FBID": "34322423", "ts": 1468308340, "type": "event", "evtName": "Product viewed", "evtData": { "Product name": "Casio Chronograph Watch", "Category": "Mens Watch", "Price": 59.99, "Currency": "USD" } } ] }' "https://api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```

## Example Response

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

## Debugging

Requests with processing errors will be returned in the API call response as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.\
A list of errors code for this API can be found on the [errors page](doc:api-errors).

## Notes

* Notification Sent, Notification Viewed, Notification Clicked, UTM Visited, App Launched, App Uninstalled, and Stayed are reserved event names.
* For the Charged event, its value object may optionally contain a nested array element called Items which may further contain value objects. The Items array is limited to 16 elements.
* Restricted characters in event and property names include dot(.), colon(;), dollar sign($), single quote('), double quote("), and backslash(\).
* Calls to this endpoint are synchronous. We recommend making individual calls, waiting for our response, and then make another request.
* Concurrent requests are limited to 3 per account. Requests that exceed the limit will return a 429 HTTP response code.

# Next Steps

Now that you’ve learned how to upload events with the CleverTap API, learn how to use the [Get Event Count API](doc:get-event-count-api) to get aggregate metrics for specific event types.

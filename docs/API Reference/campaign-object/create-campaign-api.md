---
title: Create Campaign API
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
The Create Campaign API lets you send messages to your users. 

# Overview

You can specify the audience for your message in two ways. The first option is to target people based on user events and properties. The second option is to target people based on their user ids.

For example, you can use this API to send a price drop alert as a [push notification](https://docs.clevertap.com/docs/push) to users who have viewed a certain product in the past week. If you have thousands of products and hundreds of thousands of users, this API lets you create a scalable and targeted way to notify your users of price drop alerts.

# Create Campaign API - Target User Events & Properties

To create a campaign, you have to specify the message, channel, and people to target by their user events and properties. For example, a user event could be a purchase event and the user property could be their location.

This endpoint is limited to 1 request per second.

## Base URL

[https://api.clevertap.com/1/targets/create.json](https://api.clevertap.com/1/targets/create.json)

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


The body is uploaded as a JSON payload. name, when, title, body, subject, sender\_name, target\_mode, and where are required parameters. The where parameter is an object that has optional child properties including event\_name, from, to, and profile\_fields.


<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Required
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
        name
      </td>

      <td>
        The name of your campaign that will be shown in the CleverTap dashboard.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “My Campaign”
      </td>
    </tr>

    <tr>
      <td>
        when
      </td>

      <td>
        When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “now”
      </td>
    </tr>

    <tr>
      <td>
        content
      </td>

      <td>
        Object that defines the content for your message.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "content": \{\
                "subject": "Welcome",\
                "body": "<div>Your HTML content for the email</div>",\
                "sender\_name": "CleverTap"\
            \}
      </td>
    </tr>

    <tr>
      <td>
        content.title
      </td>

      <td>
        Title content of your push notification message & web push message which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Hi”
      </td>
    </tr>

    <tr>
      <td>
        content.body
      </td>

      <td>
        Body content of your push notification, web push, email and sms which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Have you seen the special offer?”
      </td>
    </tr>

    <tr>
      <td>
        content.subject
      </td>

      <td>
        Subject content of your email which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Subject”
      </td>
    </tr>

    <tr>
      <td>
        content.sender\_name
      </td>

      <td>
        Sender name for your email which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Onboarding”
      </td>
    </tr>

    <tr>
      <td>
        target\_mode
      </td>

      <td>
        Allows you to filter which channel you want to use to send out the message. It can be “webpush”, “push”, “email”, or “sms”.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “sms”
      </td>
    </tr>

    <tr>
      <td>
        provider\_nick\_name
      </td>

      <td>
        Allows you to specify which vendor/provider to use for your campaign. For each target\_mode, you can have multiple providers integrated with CleverTap, and this parameter lets you select which provider to use for the campaign. For example, let's say you select target\_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider\_nick\_name = SendGrid to send the campaign through SendGrid.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "Provider 1"
      </td>
    </tr>

    <tr>
      <td>
        where
      </td>

      <td>
        Allows you to filter of target base by user events and profile properties. Send an empty object (\{\}) to target your entire user base.
      </td>

      <td>
        required
      </td>

      <td>
        object
      </td>

      <td>
        "where": \{\
                "event\_name": "Charged",\
                "from": 20171001,\
                "to": 20171220,\
              "common\_profile\_properties": \{\
                    "profile\_fields": [  
                        \{  
                            "name": "Customer Type",  
                            "operator":"equals",  
                            "value": "Platinum"  
                        \}  
                    ]\
                \}\
            \}
      </td>
    </tr>

    <tr>
      <td>
        where.event\_name
      </td>

      <td>
        Allows you to target users based on an event they have performed.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "Charged"
      </td>
    </tr>

    <tr>
      <td>
        where.from
      </td>

      <td>
        Start of date range for event needed. Value specified in format YYYYMMDD.
      </td>

      <td>
        optional
      </td>

      <td>
        int
      </td>

      <td>
        20171001
      </td>
    </tr>

    <tr>
      <td>
        where.to
      </td>

      <td>
        End of date range for event needed. Value specified in format YYYYMMDD.
      </td>

      <td>
        optional
      </td>

      <td>
        int
      </td>

      <td>
        20171220
      </td>
    </tr>

    <tr>
      <td>
        where.profile\_fields
      </td>

      <td>
        Allows you to target users based on the values of their profile fields.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "common\_profile\_properties": \{\
                    "profile\_fields": [  
                        \{  
                            "name": "Customer Type",  
                            "operator":"equals",  
                            "value": "Platinum"  
                        \}  
                    ]\
                \}
      </td>
    </tr>

    <tr>
      <td>
        notification\_sent
      </td>

      <td>

      </td>

      <td>
        optional
      </td>

      <td>

      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        respect\_frequency\_caps
      </td>

      <td>
        Set to false if you want to override frequency caps.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        estimate\_only
      </td>

      <td>
        If this parameter is set to true the request will return an estimated reach of the campaign, which is the number of users who will get the notification when you send it out. Setting this parameter to true will not create the campaign.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        false
      </td>
    </tr>
  </tbody>
</Table>
Below are example payloads for a web push notification on Chrome, mobile push notification, email, and SMS.


```json Web Push Notification
{
    "name": "My Webpush API campaign",
    "estimate_only": true,
    "target_mode":"webpush",
    "where": {
        "event_name": "Charged",
        "from": 20171001,
        "to": 20171220, 
      "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Customer Type",
                    "operator":"equals",
                    "value": "Platinum"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    "content": {
        "title": "Hi!",
        "body": "How are you doing today?",
        "platform_specific": {
            "chrome": {
              "image": "https://www.exampleImage.com",
              "icon":"https://www.exampleIcon.com",
              "deep_link": "http://www.example.com",
              "ttl":10,
              "require_interaction":true,
              "cta_title1":"title",
              "cta_link1":"http://www.example2.com",
              "cta_iconlink1":"https://www.exampleIcon2.com"
            }
        }
    },
    "when": "now"
}
```
```json Mobile Push Notification
{
    "name": "My API Campaign",
    "estimate_only": true,
    "target_mode":"push",
    "where": {
        "event_name": "App Launched",
        "from": 20150101,
        "to": 20150303,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Customer Type",
                    "operator":"equals",
                    "value": "Platinum"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    "content": {
        "title": "Hi!",
        "body": "How are you doing today?",
        "platform_specific": {
            "windows": {
                "deep_link": "example.com",
                "Key": "Value_windows"
            },
            "ios": {
                "mutable-content": "true", 
                "deep_link": "example.com",
                "sound_file": "example.caf",
                "category": "application category//Books",
                "badge_count": 1,
                "Key": "Value_ios"
            },
            "android": {
                "background_image": "http://example.jpg",
                "default_sound": true,
                "deep_link": "example.com",
                "large_icon": "http://example.png",
                "Key": "Value_android"
            }
        }
    },
    "devices": [
        "android",
        "ios",
        "windows"
    ],
    "when": "now"
}
```
```json Email
{
    "name": "My Email API campaign",
    "estimate_only": true,
    "target_mode":"email",
    "where": {
        "event_name": "Charged",
        "from": 20171001,
        "to": 20171220, 
      "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Customer Type",
                    "operator":"equals",
                    "value": "Platinum"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    "content": {
        "subject": "Welcome",
        "body": "<div>Your HTML content for the email</div>",
        "sender_name": "CleverTap"
    },
    "when": "now"
}
```
```json SMS
{
    "name": "My Sms API campaign",
    "estimate_only": true,
    "target_mode":"sms",
    "where": {
        "event_name": "Charged",
        "from": 20171001,
        "to": 20171220, 
      "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Customer Type",
                    "operator":"equals",
                    "value": "Platinum"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    "content": {
        "body": "Sms body"
    },
    "when": "now"
}
```


## Example Request

























































































































































































































































































































































































































































































































































## Example Response











































































# Create Campaign API - Target Users by their Identities

You can send notifications to your users based on their Facebook ID, Google Plus ID, Email ID, custom-defined identity, or CleverTap ID.

You can message 1000 users per API request.

## Base URL

The endpoint is different based on the channel you are sending the message. 

**SMS**\
[https://api.clevertap.com/1/send/sms.json](https://api.clevertap.com/1/send/sms.json)

**Push**\
[https://api.clevertap.com/1/send/push.json](https://api.clevertap.com/1/send/push.json)

**Web Push**\
[https://api.clevertap.com/1/send/webpush.json](https://api.clevertap.com/1/send/webpush.json)

**Email**\
[https://api.clevertap.com/1/send/email.json](https://api.clevertap.com/1/send/email.json)

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
        Request content-type should always set to application/json; charset=utf-8.
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

The body is uploaded as a JSON payload. 

to, name, when, title, body, subject, sender\_name, and  target\_mode are required parameters. 

The to parameter is an object that has the following child properties: FBID, GPID, Email, Identity, objectId, which are used as identifiers to target your users. Each request can contain a max of 1000 users across these identifiers. 


<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Required
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
        to
      </td>

      <td>
        Used to define which users will receive the message.
      </td>

      <td>
        required
      </td>

      <td>
        object
      </td>

      <td>
        "to": \{\
                "FBID": [  
                    "102029292929388",  
                    "114342342453463"  
                ],\
                "GPID": [  
                    "1928288389299292"  
                ],\
                "Email": [  
                    "john@doe.com",  
                    "jane@doe.com"  
                ],\
                "Identity": [  
                    "JohnDoe"  
                ],\
                "objectId": [  
                    "_asdnkansdjknaskdjnasjkndja",  
                    "-adffajjdfoaiaefiohnefwprjf"  
                ]\
            \}
      </td>
    </tr>

    <tr>
      <td>
        to.FBID
      </td>

      <td>
        Facebook ID for user.
      </td>

      <td>
        required
      </td>

      <td>
        array of strings
      </td>

      <td>
        "to": \{\
                "FBID": [  
                    "102029292929388",  
                    "114342342453463"  
                ]\
        \}
      </td>
    </tr>

    <tr>
      <td>
        to.GPID
      </td>

      <td>
        Google ID for user.
      </td>

      <td>
        required
      </td>

      <td>
        array of strings
      </td>

      <td>
        \{\
            "to": \{\
                "GPID": [  
                    "102029292929388",  
                    "114342342453463"  
                ]\
        \}
      </td>
    </tr>

    <tr>
      <td>
        to.Email
      </td>

      <td>
        Email identifier for user.
      </td>

      <td>
        required
      </td>

      <td>
        array of strings
      </td>

      <td>
        \{\
            "to": \{\
                "Email": [  
                    "john@doe.com",  
                    "jan@doe.com"  
                ]\
        \}
      </td>
    </tr>

    <tr>
      <td>
        to.Identity
      </td>

      <td>
        Custom-defined identifier for user.
      </td>

      <td>
        required
      </td>

      <td>
        array of strings
      </td>

      <td>
        \{\
            "to": \{\
              "Identity": [  
                    "JohnDoe"  
                ]\
        \}
      </td>
    </tr>

    <tr>
      <td>
        to.objectId
      </td>

      <td>
        CleverTap identifier for user.
      </td>

      <td>
        required
      </td>

      <td>
        array of strings
      </td>

      <td>
        \{\
            "to": \{\
                "objectId": [  
         "sdnkansdjknaskdjnasjkndja",     "dffajjdfoaiaefiohnefwprjf"  
                ]\
        \}
      </td>
    </tr>

    <tr>
      <td>
        tag\_group
      </td>

      <td>
        Each message can be tagged into a tag group. Metrics, such as sent and viewed, will be shown against each tag group in the CleverTap dashboard. Max 20 groups. If not specified, the merics will be shown against the “Not Tagged” tag.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "my tag group"
      </td>
    </tr>

    <tr>
      <td>
        when
      </td>

      <td>
        When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “now”
      </td>
    </tr>

    <tr>
      <td>
        content
      </td>

      <td>
        Object that defines the content for your message.
      </td>

      <td>
        required
      </td>

      <td>
        object
      </td>

      <td>
        "content": \{\
                "subject": "Welcome",\
                "body": "<div>Your HTML content for the email</div>",\
                "sender\_name": "CleverTap"\
            \}
      </td>
    </tr>

    <tr>
      <td>
        content.title
      </td>

      <td>
        Title content of your push notification message and web push message which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "Hi"
      </td>
    </tr>

    <tr>
      <td>
        content.body
      </td>

      <td>
        Body content of your push notification, web push, email and sms which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Have you seen the special offer?”
      </td>
    </tr>

    <tr>
      <td>
        content.subject
      </td>

      <td>
        Subject content of your email which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "Subject"
      </td>
    </tr>

    <tr>
      <td>
        content.sender\_name
      </td>

      <td>
        Sender name for your email which is sent to the user.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Onboarding”
      </td>
    </tr>

    <tr>
      <td>
        provider\_nick\_name
      </td>

      <td>
        Allows you to specify which vendor/provider to use for your campaign. For each target\_mode, you can have multiple providers integrated with CleverTap, and this parameter lets you select which provider to use for the campaign. For example, let's say you select target\_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider\_nick\_name = SendGrid to send the campaign through SendGrid.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "Provider 1"
      </td>
    </tr>

    <tr>
      <td>
        notification\_sent
      </td>

      <td>

      </td>

      <td>
        optional
      </td>

      <td>

      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        respect\_frequency\_caps
      </td>

      <td>
        Set to false if you want to override frequency caps.
      </td>

      <td>
        optional
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
        estimate\_only
      </td>

      <td>
        If this parameter is set to true the request will return an estimated reach of the campaign, which is the number of users who will get the notification when you send it out. Setting this parameter to true will not create the campaign.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        false
      </td>
    </tr>
  </tbody>
</Table>


Below are example payloads for web push, push, email, and SMS.


```json Web Push
{
    "to": {
        "FBID": [
            "102029292929388",
            "114342342453463"
        ],
        "GPID": [
            "1928288389299292"
        ],
        "Email": [
            "john@doe.com",
            "jane@doe.com"
        ],
        "Identity": [
            "JohnDoe"
        ],
        "objectId": [
            "_asdnkansdjknaskdjnasjkndja",
            "-adffajjdfoaiaefiohnefwprjf"
        ]
    },
    "tag_group": "my tag group",
    "respect_frequency_caps": false,
     "content": {
        "title": "Hi!",
        "body": "How are you doing today?",
        "platform_specific": {
            "chrome": {
                "image": "https://www.exampleImage.com",
              "icon":"https://www.exampleIcon.com",
                "deep_link": "http://www.example.com",
              "ttl":10,
              "require_interaction":true,
              "cta_title1":"title",
              "cta_link1":"http://www.example2.com",
              "cta_iconlink1":"https://www.exampleIcon2.com"
            }
        }
    }
}
```
```json Push
{
    "to": {
        "FBID": [
            "102029292929388",
            "114342342453463"
        ],
        "GPID": [
            "1928288389299292"
        ],
        "Email": [
            "john@doe.com",
            "jane@doe.com"
        ],
        "Identity": [
            "JohnDoe"
        ],
        "objectId": [
            "_asdnkansdjknaskdjnasjkndja",
            "-adffajjdfoaiaefiohnefwprjf"
        ]
    },
    "tag_group": "my tag group",
    "respect_frequency_caps": false,
    "content": {
        "title": "Welcome",
        "body": "Hello world!",
        "platform_specific": {
            "windows": {
                "deep_link": "example.com",
                "key": "value_windows"
            },
            "ios": {
                "deep_link": "example.com",
                "sound_file": "example.caf",
                "category": "notification category",
                "badge_count": 1,
                "key": "value_ios"
            },
            "android": {
                "background_image": "http://example.jpg",
                "default_sound": true,
                "deep_link": "example.com",
                "large_icon": "http://example.png",
                "key": "value_android"
            }
        }
    }
}
```
```json Email
{
    "to": {
        "FBID": [
            "102029292929388",
            "114342342453463"
        ],
        "GPID": [
            "1928288389299292"
        ],
        "Email": [
            "john@doe.com",
            "jane@doe.com"
        ],
        "Identity": [
            "JohnDoe"
        ],
        "objectId": [
            "_asdnkansdjknaskdjnasjkndja",
            "-adffajjdfoaiaefiohnefwprjf"
        ]
    },
    "tag_group": "my tag group",
    "respect_frequency_caps": false,
    "content": {
        "subject": "Welcome",
        "body": "<div>Your HTML content for the email</div>",
        "sender_name": "CleverTap"
    }
}
```
```json SMS
{
    "to": {
        "FBID": [
            "102029292929388",
            "114342342453463"
        ],
        "GPID": [
            "1928288389299292"
        ],
        "Email": [
            "john@doe.com",
            "jane@doe.com"
        ],
        "Identity": [
            "JohnDoe"
        ],
        "objectId": [
            "_asdnkansdjknaskdjnasjkndja",
            "-adffajjdfoaiaefiohnefwprjf"
        ]
    },
    "tag_group": "my tag group",
    "respect_frequency_caps": false,
     "content": {
        "body": "Sms body"
    }
}
```


## Example Request






















































































































































































































































































































































































































































































































## Example Response


























































































A success response implies payload validity and addition to the processing queue. Message delivery is handled by the provider (push or email) and depends on if you provided a valid recipient id.

## Notes

If there are errors, you will receive a response in the format below. 











































































































Here are the possible error codes.\
21 – to is a mandatory field\
84 – Invalid recipients”\
73 – “respect\_frequency\_caps” must be a Boolean\
78 – Invalid JSON payload\
89 – Unexpected error, please try again

# Unsubscribe Link for Emails

If you’re using Amazon SES or your own SMTP gateway, you’ll have to follow the steps [listed on this page](https://docs.clevertap.com/docs/handling-unsubscribes) to enable your users to unsubscribe from your email notifications.

# Next Steps

Now that you’ve learned how send a notification from the CleverTap API, learn how to get metrics about a campaign using the [Get Campaign Report API](doc:get-campaign-report-api).
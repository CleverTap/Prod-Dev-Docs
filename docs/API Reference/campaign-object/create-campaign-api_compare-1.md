---
title: Create Campaign API_Compare
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
The Create Campaign API lets you send messages to your users. Compare text and images

# Overview

You can specify the audience for your message in two ways. The first option is to target people based on user events and properties. The second option is to target people based on their user ids.

For example, you can use this API to send a price drop alert as a [push notification](https://docs.clevertap.com/docs/push) to users who have viewed a certain product in the past week. If you have thousands of products and hundreds of thousands of users, this API lets you create a scalable and targeted way to notify your users of price drop alerts.

# Create Campaign API - Target User Events & Properties

To create a campaign, you have to specify the message, channel, and people to target by their user events and properties. For example, a user event could be a purchase event and the user property could be their location.

This endpoint is limited to 1 request per second.

## Base URL - Target User Events & Properties

[https://api.clevertap.com/1/targets/create.json](https://api.clevertap.com/1/targets/create.json)

## HTTP Method - Target User Events & Properties

POST

## Headers - Target User Events & Properties

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


## Body Parameters - Target User Events & Properties

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
            }
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
        Allows you to filter of target base by user events and profile properties. Send an empty object (\{}) to target your entire user base.
      </td>

      <td>
        required
      </td>

      <td>
        object
      </td>

      <td>
        <code>{`"where": { "event_name": "Charged", "from": 20171001, "to": 20171220, "common_profile_properties": { "profile_fields": [{ "name": "Customer Type", "operator": "equals", "value": "Platinum" }] } }`}</code>
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
        <code>{`"common_profile_properties": { "profile_fields": [{ "name": "Customer Type", "operator": "equals", "value": "Platinum" }] }`}</code>
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
        If this parameter is set to true the request will return an estimated reach of the campaign, which is the number of users who will get the notification when you send it out. Setting this parameter to true will not create the campaign. For an asynchronous call, the async\_estimate flag must be passed as true.
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
        async\_estimate
      </td>

      <td>
        This allows setting of the estimate\_only flag in the asynchronously. If this parameter is set to true, it will return a request ID that can be passed in the subsequent API call.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        ttl
      </td>

      <td>
        This allows setting of time of live for push notifications for android. The value has to be entered in hours which can range from 0 to 672 (i.e. 28 days). Default is 24 hours
      </td>

      <td>
        optional
      </td>

      <td>
        int
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        wzrk\_cid
      </td>

      <td>
        Platform specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.

        If a Notification ID is mandatory then the channel ID is expected. An error occurs if a valid channel ID is not received.
      </td>

      <td>
        optional / mandatory (Configurable)
      </td>

      <td>
        string
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        wzrk\_bc
      </td>

      <td>
        Platform specific key that can be used while targeting users on devices running Android O - can be used to determine the notification count shown on the app icon
      </td>

      <td>
        optional
      </td>

      <td>
        int
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        wzrk\_bi
      </td>

      <td>
        Platform specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.

        Accepted values\
        `app icon`\
        `notification icon`
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        system\_control\_group\_include = true
      </td>

      <td>
        If the system control group is enabled from the dashboard, you can choose to use the control group for API campaigns
      </td>

      <td>
        optional
      </td>

      <td>
        boolean (true/false)
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        "control\_group": \{\
        "type": "custom",\
        "name": "*name of control group*"\
        }
      </td>

      <td>
        If the custom control group is enabled from the dashboard, you can choose to use the control group for API campaigns
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        "control\_group": \{\
        "type": "campaign",\
        "percentage": 5\
        }
      </td>

      <td>
        You can create a campaign control group via API by specifying the % as described on the left
      </td>

      <td>
        optional
      </td>

      <td>
        integer
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        <code>{`wzrk_acts: "[{\"l\":\"Action-1\",\"dl\":\"\",\"id\":\"01\",\"ico\":\"\"},{"l":"Action-2","dl":"","id":"02","ico":""},{"l":"Action-3","dl":"","id":"03","ico":""}]"`}</code>
      </td>

      <td>
        Android Action Buttons\
        "l"= Action Title, "dl" = Deep link, "id" = Action id, "ico" = icon resource identifier
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        "respect\_throttle":true
      </td>

      <td>
        Toggle this flag to true if you want to enable throttle limits for the campaign.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        "skip\_estimate":true
      </td>

      <td>
        Toggle this flag to true if you do not want to estimate the reach. This will lead to faster campaign delivery.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>

      </td>
    </tr>
  </tbody>
</Table>
Example Payloads : 


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
                "Key": "Value_android",
                "ttl":24
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
```json Webhooks
{
    "name": "My Webhook Campaign",
    "estimate_only": true,
    "target_mode":"webhook",
    "where": {
        "event_name": "Charged",
        "from": 20190101,
        "to": 20191220
    },
    "respect_frequency_caps": false,
    "webhook_endpoint_name":"Recommendation Endpoint",
    "webhook_fields":["profile-attributes","tokens","identities"],
    "webhook_key_value" : {
        "k1":"v1",
        "k2":"v2"
    },
    "when": "now"
}
```
```json App Inbox - Simple Message
{    
    "name": "api simple test",
    "estimate_only": false,
    "target_mode":"notificationinbox",
    "where": {
        "event_name": "Charged",
        "from": 20150101,
        "to": 20150303
    },
    "respect_frequency_caps": false,
    "template_type":"simple", // mandatory
    "content": [
        {
            "message":{
                "text":"MessageText"
            },
            "title":{
                "text":"titleText"
            },
            "action":{    
                "url":{
                    "android":{
                        "text":"andmurl"    
                    },
                    "ios":{
                        "text":"iosmurl"    
                    }
                },
                "links":[
                    {
                        "type":"copy",
                        "text":"linkText",
                        "copyText":{
                            "text":"copyText"
                        }
                    },
                    {
                        "type":"url",
                        "text":"linkText",
                        "url":{
                            "android":{
                                "text":"andLinkurl"    
                            },
                            "ios":{
                                "text":"iosLinkurl"
                            }
                        }    
                    }
                    ]
            },
            "media":{
                "content_type":"image/jpeg",
                "url":"example.com"
            }
        }
        ],
    "time_to_live":50,    
    "tags":[
        "abc",
        "bcd"
        ],
    "devices": [
        "ios",
        "android"
    ],
    "when": "now"
}
```
```json App Inbox - Carousel
{    
    "name": "api simple test",
    "estimate_only": false,
    "target_mode":"notificationinbox",
    "where": {
        "event_name": "Charged",
        "from": 20150101,
        "to": 20150303
    },
    "respect_frequency_caps": false,
    "template_type":"carousel",
    "orientation": "p",
    "content": [
        {
            "message":{
                "text":"Message1"
            },
            "title":{
                "text":"title1"
            },
            "action":{
                "url":{
                    "android":{
                        "text":"andurl1"
                    },
                    "ios":{
                        "text":"iosmurl1"    
                    }
                }
            },
            "media":{
                "content_type":"image/jpg",
                "url":"example.com"
            }
        },
        {
            "message":{
                "text":"Message2"
            },
            "title":{
                "text":"title2"
            },
            "action":{
                "url":{
                    "android":{
                        "text":"andurl2"
                    },
                    "ios":{
                        "text":"iosmurl2"    
                    }
                }
            },
            "media":{
                "content_type":"image/jpg",
                "url":"example.com"
            }
        }
        ],
    "time_to_live":8,
    "tags":[
        "abc",
        "bcd"
        ],
    "devices": [
        "ios",
        "android"
    ],
    "when": "now"
}


// media & template_type is mandatory
```
```json App Inbox - Carousel Image
{    
    "name": "api simple test",
    "estimate_only": false,
    "target_mode":"notificationinbox",
    "where": {
        "event_name": "Charged",
        "from": 20150101,
        "to": 20150303
    },
    "respect_frequency_caps": false,
    "template_type":"carousel-image",
    "orientation": "p",
    "content": [
        {
            "action":{
                "url":{
                    "android":{
                        "text":"andurl1"
                    },
                    "ios":{
                        "text":"iosmurl1"    
                    }
                }
            },
            "media":{
                "content_type":"image/jpg",
                "url":"example.com"
            }
        },
        {
            "message":{
                "text":"Message2"
            },
            "title":{
                "text":"title2"
            },
            "action":{
                "url":{
                    "android":{
                        "text":"andurl2"
                    },
                    "ios":{
                        "text":"iosmurl2"    
                    }
                }
            },
            "media":{
                "content_type":"image/jpg",
                "url":"example.com"
            }
        }
        ],
    "time_to_live":8,
    "tags":[
        "abc",
        "bcd"
        ],
    "devices": [
        "ios",
        "android"
    ],
    "when": "now"
}


// media & template_type is mandatory
```
```json App Inbox - Message with icon
{    
    "name": "api app inbox icon",
    "estimate_only": false,
    "target_mode":"notificationinbox",
    "where": {
        "event_name": "App Launched",
        "from": 20190101,
        "to": 20190418
    },
    "respect_frequency_caps": false,
    "template_type":"message-icon",
    "orientation": "p",
    "content": [
        {
            "message":{
                "text":"MessageTexticon"
            },
            "title":{
                "text":"titleTextIcon"
            },
            "action":{
                "url":{
                    "android":{
                        "text":"andmurl"
                    },
                    "ios":{
                        "text":"iosmurl"    
                    }
                },
                "links":[
                    {
                        "type":"url",
                        "text":"linktextIcon",
                        "color":"#000000",
                        "bg":"#ffff",
                        "url":{
                            "android":{
                                "text":"andlurl"
                            },
                            "ios":{
                                "text":"ioslurl"
                            }
                        }
                    }
                    ]
            },
            "media":{
                "content_type":"image/jpg",
                "url":"https://s3.amazonaws.com/ct-demo-images/landscape-2.jpg"
            },
            "icon":{
                "content_type":"image/jpg",
                "url":"https://s3.amazonaws.com/ct-demo-images/landscape-2.jpg"
            }
        }
        ],
    "time_to_live":50,    
   
    "devices": [
        "ios",
        "android"
    ],
    "when": "now"
}

// template_type is mandatory
```


## Notes

The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req\_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful or a failed response with an error string and an HTTP status code.

Here is an example response for a partial status.





































































After getting the req\_id, you will poll the endpoint below and provide the value of req\_id as a query parameter.

GET [https://api.clevertap.com/1/counts/create.json?req\_id=](https://api.clevertap.com/1/counts/create.json?req_id=)\<your\_request\_id\_here>

# Next Steps

Now that you’ve learned how to get a message report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).

## Example Request - Target User Events & Properties


```curl
curl -X POST -d ' {"name": "My Sms API campaign","estimate_only": true,"target_mode":"sms", "where":{"event_name":"Charged","from":20171001,"to":20171220,"common_profile_properties":{"profile_fields": [ {"name": "Customer Type","operator":"equals","value": "Platinum"}]}},"respect_frequency_caps": false,"content": { "body": "Sms body"},"when": "now"}' \
"https://api.clevertap.com/1/targets/create.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/targets/create.json ")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "name" => "My Sms API campaign",
  "estimate_only" => true,
  "target_mode" => "sms",
  "where" => {
    "event_name" => "Charged",
    "from" => 20171001,
    "to" => 20171220,
    "common_profile_properties" => {
      "profile_fields" => [
        {
          "name" => "Customer Type",
          "operator" => "equals",
          "value" => "Platinum"
        }
      ]
    }
  },
  "respect_frequency_caps" => false,
  "content" => {
    "body" => "Sms body"
  },
  "when" => "now"
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

data = ' {"name": "My Sms API campaign","estimate_only": true,"target_mode":"sms", "where":{"event_name":"Charged","from":20171001,"to":20171220,"common_profile_properties":{"profile_fields": [ {"name": "Customer Type","operator":"equals","value": "Platinum"}]}},"respect_frequency_caps": false,"content": { "body": "Sms body"},"when": "now"}'

response = requests.post('https://api.clevertap.com/1/targets/create.json', headers=headers, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$data = ' {"name": "My Sms API campaign","estimate_only": true,"target_mode":"sms", "where":{"event_name":"Charged","from":20171001,"to":20171220,"common_profile_properties":{"profile_fields": [ {"name": "Customer Type","operator":"equals","value": "Platinum"}]}},"respect_frequency_caps": false,"content": { "body": "Sms body"},"when": "now"}';
$response = Requests::post('https://api.clevertap.com/1/targets/create.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = ' {"name": "My Sms API campaign","estimate_only": true,"target_mode":"sms", "where":{"event_name":"Charged","from":20171001,"to":20171220,"common_profile_properties":{"profile_fields": [ {"name": "Customer Type","operator":"equals","value": "Platinum"}]}},"respect_frequency_caps": false,"content": { "body": "Sms body"},"when": "now"}';

var options = {
    url: 'https://api.clevertap.com/1/targets/create.json ',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```


## Example Response - Target User Events & Properties











































































# Create Campaign API - Target Users by their Identities

You can send notifications to your users based on their Facebook ID, Google Plus ID, Email ID, custom-defined identity, or CleverTap ID.

You can message 1000 users per API request.

## Base URL - Target Users by their Identities

The endpoint is different based on the channel you are sending the message. 

**SMS**\
[https://api.clevertap.com/1/send/sms.json](https://api.clevertap.com/1/send/sms.json)

**Push**\
[https://api.clevertap.com/1/send/push.json](https://api.clevertap.com/1/send/push.json)

> ❗️ The push.json API currently does not support Push Amplification.

**Web Push**\
[https://api.clevertap.com/1/send/webpush.json](https://api.clevertap.com/1/send/webpush.json)

**Email**\
[https://api.clevertap.com/1/send/email.json](https://api.clevertap.com/1/send/email.json)

## HTTP Method - Target Users by their Identities

POST

## Headers - Target Users by their Identities

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
        "Content-Type: application/json;charset=utf-8"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters - Target Users by their Identities

The body is uploaded as a JSON payload. 


to, name, title, body, subject, sender\_name, and  target\_mode are required parameters. 

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
        <code>{`"to": { "FBID": ["102029292929388", "114342342453463"], "GPID": ["1928288389299292"], "Email": ["john@doe.com", "jane@doe.com"], "Identity": ["JohnDoe"], "objectId": ["_asdnkansdjknaskdjnasjkndja", "-adffajjdfoaiaefiohnefwprjf"] }`}</code>
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
        }
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
        }
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
        }
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
        <code>{`{ "to": { "Identity": ["JohnDoe"] } }`}</code>
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
        }
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
        optional (either use tag \_group or campaign\_id)
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
            }
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
        wzrk\_cid
      </td>

      <td>
        Platform specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.

        If a Notification ID is mandatory then the channel ID is expected. An error occurs if a valid channel ID is not received.
      </td>

      <td>
        optional / mandatory (configurable)
      </td>

      <td>
        string
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        badge\_id
      </td>

      <td>
        Platform specific key that can be used while targeting users on devices running Android O - can be used to determine the notification count shown on the app icon
      </td>

      <td>
        optional
      </td>

      <td>
        int
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        badge\_icon
      </td>

      <td>
        Platform specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.

        Accepted values\
        `app icon`\
        `notification icon`
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        mutable-content
      </td>

      <td>
        To raise Notification viewed event for Push notifications, raise the flag to "true" in the platform\_specific: ios section of the request payload.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>

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
		"campaign_id": 1000000043,
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


## Example Request - Target Users by their Identities


```curl
curl -X POST -d '{"to":{"FBID":["102029292929388","114342342453463"],"GPID":["1928288389299292"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"campaign_id":1000000043,"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}' \
"https://api.clevertap.com/1/send/sms.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/send/sms.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "to" => {
    "FBID" => [
      "102029292929388",
      "114342342453463"
    ],
    "GPID" => [
      "1928288389299292"
    ],
    "Email" => [
      "john@doe.com",
      "jane@doe.com"
    ],
    "Identity" => [
      "JohnDoe"
    ],
    "objectId" => [
      "_asdnkansdjknaskdjnasjkndja",
      "-adffajjdfoaiaefiohnefwprjf"
    ]
  },
  "tag_group" => "mytaggroup",
  "respect_frequency_caps" => false,
  "content" => {
    "body" => "Smsbody"
  }
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

data = '{"to":{"FBID":["102029292929388","114342342453463"],"GPID":["1928288389299292"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}'

response = requests.post('https://api.clevertap.com/1/send/sms.json', headers=headers, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$data = '{"to":{"FBID":["102029292929388","114342342453463"],"GPID":["1928288389299292"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}';
$response = Requests::post('https://api.clevertap.com/1/send/sms.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"to":{"FBID":["102029292929388","114342342453463"],"GPID":["1928288389299292"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}';

var options = {
    url: 'https://api.clevertap.com/1/send/sms.json',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```


## Example Response - Target Users by their Identities


























































































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
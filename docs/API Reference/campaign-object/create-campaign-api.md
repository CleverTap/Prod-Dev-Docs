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
https://api.clevertap.com/1/targets/create.json

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "0-0": "X-CleverTap-Account-Id",
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
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
The body is uploaded as a JSON payload. name, when, title, body, subject, sender_name, target_mode, and where are required parameters. The where parameter is an object that has optional child properties including event_name, from, to, and profile_fields.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "name",
    "1-0": "when",
    "2-0": "content",
    "3-0": "content.title",
    "4-0": "content.body",
    "5-0": "content.subject",
    "6-0": "content.sender_name",
    "7-0": "target_mode",
    "9-0": "where",
    "10-0": "where.event_name",
    "11-0": "where.from",
    "12-0": "where.to",
    "13-0": "where.profile_fields",
    "14-0": "notification_sent",
    "15-0": "respect_frequency_caps",
    "16-0": "estimate_only",
    "16-1": "If this parameter is set to true the request will return an estimated reach of the campaign, which is the number of users who will get the notification when you send it out. Setting this parameter to true will not create the campaign.",
    "15-1": "Set to false if you want to override frequency caps.",
    "13-1": "Allows you to target users based on the values of their profile fields.",
    "12-1": "End of date range for event needed. Value specified in format YYYYMMDD.",
    "11-1": "Start of date range for event needed. Value specified in format YYYYMMDD.",
    "10-1": "Allows you to target users based on an event they have performed.",
    "9-1": "Allows you to filter of target base by user events and profile properties. Send an empty object ({}) to target your entire user base.",
    "7-1": "Allows you to filter which channel you want to use to send out the message. It can be “webpush”, “push”, “email”, or “sms”.",
    "6-1": "Sender name for your email which is sent to the user.",
    "5-1": "Subject content of your email which is sent to the user.",
    "4-1": "Body content of your push notification, web push, email and sms which is sent to the user.",
    "3-1": "Title content of your push notification message & web push message which is sent to the user.",
    "2-1": "Object that defines the content for your message.",
    "1-1": "When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.",
    "0-1": "The name of your campaign that will be shown in the CleverTap dashboard.",
    "0-2": "required",
    "1-2": "required",
    "2-2": "required",
    "3-2": "required",
    "4-2": "required",
    "5-2": "required",
    "6-2": "required",
    "7-2": "required",
    "9-2": "required",
    "10-2": "optional",
    "11-2": "optional",
    "12-2": "optional",
    "13-2": "optional",
    "14-2": "optional",
    "15-2": "optional",
    "16-2": "optional",
    "0-3": "string",
    "1-3": "string",
    "2-3": "string",
    "3-3": "string",
    "4-3": "string",
    "5-3": "string",
    "6-3": "string",
    "7-3": "string",
    "9-3": "object",
    "10-3": "string",
    "11-3": "int",
    "12-3": "int",
    "13-3": "string",
    "15-3": "boolean",
    "16-3": "boolean",
    "0-4": "“My Campaign”",
    "1-4": "“now”",
    "2-4": "\"content\": {\n        \"subject\": \"Welcome\",\n        \"body\": \"<div>Your HTML content for the email</div>\",\n        \"sender_name\": \"CleverTap\"\n    }",
    "3-4": "“Hi”",
    "4-4": "“Have you seen the special offer?”",
    "5-4": "“Subject”",
    "6-4": "“Onboarding”",
    "7-4": "“sms”",
    "9-4": "\"where\": {\n        \"event_name\": \"Charged\",\n        \"from\": 20171001,\n        \"to\": 20171220, \n      \"common_profile_properties\": {\n            \"profile_fields\": [\n                {\n                    \"name\": \"Customer Type\",\n                    \"operator\":\"equals\",\n                    \"value\": \"Platinum\"\n                }\n            ]\n        }\n    }",
    "10-4": "\"Charged\"",
    "11-4": "20171001",
    "12-4": "20171220",
    "13-4": "\"common_profile_properties\": {\n            \"profile_fields\": [\n                {\n                    \"name\": \"Customer Type\",\n                    \"operator\":\"equals\",\n                    \"value\": \"Platinum\"\n                }\n            ]\n        }",
    "15-4": "true",
    "16-4": "false",
    "8-0": "provider_nick_name",
    "8-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter lets you select which provider to use for the campaign. For example, let's say you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid.",
    "8-2": "optional",
    "8-3": "string",
    "8-4": "\"Provider 1\""
  },
  "cols": 5,
  "rows": 17
}
[/block]
Below are example payloads for a web push notification on Chrome, mobile push notification, email, and SMS.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"name\": \"My Webpush API campaign\",\n    \"estimate_only\": true,\n    \"target_mode\":\"webpush\",\n    \"where\": {\n        \"event_name\": \"Charged\",\n        \"from\": 20171001,\n        \"to\": 20171220, \n      \"common_profile_properties\": {\n            \"profile_fields\": [\n                {\n                    \"name\": \"Customer Type\",\n                    \"operator\":\"equals\",\n                    \"value\": \"Platinum\"\n                }\n            ]\n        }\n    },\n    \"respect_frequency_caps\": false,\n    \"content\": {\n        \"title\": \"Hi!\",\n        \"body\": \"How are you doing today?\",\n        \"platform_specific\": {\n            \"chrome\": {\n              \"image\": \"https://www.exampleImage.com\",\n              \"icon\":\"https://www.exampleIcon.com\",\n              \"deep_link\": \"http://www.example.com\",\n              \"ttl\":10,\n              \"require_interaction\":true,\n              \"cta_title1\":\"title\",\n              \"cta_link1\":\"http://www.example2.com\",\n              \"cta_iconlink1\":\"https://www.exampleIcon2.com\"\n            }\n        }\n    },\n    \"when\": \"now\"\n}",
      "language": "json",
      "name": "Web Push Notification"
    },
    {
      "code": " {\n    \"name\": \"My API Campaign\",\n    \"estimate_only\": true,\n    \"target_mode\":\"push\",\n    \"where\": {\n        \"event_name\": \"App Launched\",\n        \"from\": 20150101,\n        \"to\": 20150303,\n        \"common_profile_properties\": {\n            \"profile_fields\": [\n                {\n                    \"name\": \"Customer Type\",\n                    \"operator\":\"equals\",\n                    \"value\": \"Platinum\"\n                }\n            ]\n        }\n    },\n    \"respect_frequency_caps\": false,\n    \"content\": {\n        \"title\": \"Hi!\",\n        \"body\": \"How are you doing today?\",\n        \"platform_specific\": {\n            \"windows\": {\n                \"deep_link\": \"example.com\",\n                \"Key\": \"Value_windows\"\n            },\n            \"ios\": {\n                \"mutable-content\": \"true\", \n                \"deep_link\": \"example.com\",\n                \"sound_file\": \"example.caf\",\n                \"category\": \"application category//Books\",\n                \"badge_count\": 1,\n                \"Key\": \"Value_ios\"\n            },\n            \"android\": {\n                \"background_image\": \"http://example.jpg\",\n                \"default_sound\": true,\n                \"deep_link\": \"example.com\",\n                \"large_icon\": \"http://example.png\",\n                \"Key\": \"Value_android\"\n            }\n        }\n    },\n    \"devices\": [\n        \"android\",\n        \"ios\",\n        \"windows\"\n    ],\n    \"when\": \"now\"\n}",
      "language": "json",
      "name": "Mobile Push Notification"
    },
    {
      "code": "{\n    \"name\": \"My Email API campaign\",\n    \"estimate_only\": true,\n    \"target_mode\":\"email\",\n    \"where\": {\n        \"event_name\": \"Charged\",\n        \"from\": 20171001,\n        \"to\": 20171220, \n      \"common_profile_properties\": {\n            \"profile_fields\": [\n                {\n                    \"name\": \"Customer Type\",\n                    \"operator\":\"equals\",\n                    \"value\": \"Platinum\"\n                }\n            ]\n        }\n    },\n    \"respect_frequency_caps\": false,\n    \"content\": {\n        \"subject\": \"Welcome\",\n        \"body\": \"<div>Your HTML content for the email</div>\",\n        \"sender_name\": \"CleverTap\"\n    },\n    \"when\": \"now\"\n}",
      "language": "json",
      "name": "Email"
    },
    {
      "code": " {\n    \"name\": \"My Sms API campaign\",\n    \"estimate_only\": true,\n    \"target_mode\":\"sms\",\n    \"where\": {\n        \"event_name\": \"Charged\",\n        \"from\": 20171001,\n        \"to\": 20171220, \n      \"common_profile_properties\": {\n            \"profile_fields\": [\n                {\n                    \"name\": \"Customer Type\",\n                    \"operator\":\"equals\",\n                    \"value\": \"Platinum\"\n                }\n            ]\n        }\n    },\n    \"respect_frequency_caps\": false,\n    \"content\": {\n        \"body\": \"Sms body\"\n    },\n    \"when\": \"now\"\n}",
      "language": "json",
      "name": "SMS"
    }
  ]
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d ' {\"name\": \"My Sms API campaign\",\"estimate_only\": true,\"target_mode\":\"sms\", \"where\":{\"event_name\":\"Charged\",\"from\":20171001,\"to\":20171220,\"common_profile_properties\":{\"profile_fields\": [ {\"name\": \"Customer Type\",\"operator\":\"equals\",\"value\": \"Platinum\"}]}},\"respect_frequency_caps\": false,\"content\": { \"body\": \"Sms body\"},\"when\": \"now\"}' \\\n\"https://api.clevertap.com/1/targets/create.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n  \"status\": \"success\",\n  \"estimates\": {\n    \"web\": 2\n  }\n}\n",
      "language": "json"
    }
  ]
}
[/block]
# Create Campaign API - Target Users by their Identities
You can send notifications to your users based on their Facebook ID, Google Plus ID, Email ID, custom-defined identity, or CleverTap ID.

You can message 1000 users per API request.

## Base URL
The endpoint is different based on the channel you are sending the message. 

**SMS**
https://api.clevertap.com/1/send/sms.json

**Push**
https://api.clevertap.com/1/send/push.json

**Web Push**
https://api.clevertap.com/1/send/webpush.json

**Email**
https://api.clevertap.com/1/send/email.json

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
    "0-0": "X-CleverTap-Account-Id",
    "h-3": "Example Value",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type should always set to application/json; charset=utf-8.",
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

to, name, when, title, body, subject, sender_name, and  target_mode are required parameters. 

The to parameter is an object that has the following child properties: FBID, GPID, Email, Identity, objectId, which are used as identifiers to target your users. Each request can contain a max of 1000 users across these identifiers. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "to",
    "0-1": "Used to define which users will receive the message.",
    "0-2": "required",
    "0-3": "object",
    "0-4": "\"to\": {\n        \"FBID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ],\n        \"GPID\": [\n            \"1928288389299292\"\n        ],\n        \"Email\": [\n            \"john@doe.com\",\n            \"jane@doe.com\"\n        ],\n        \"Identity\": [\n            \"JohnDoe\"\n        ],\n        \"objectId\": [\n            \"_asdnkansdjknaskdjnasjkndja\",\n            \"-adffajjdfoaiaefiohnefwprjf\"\n        ]\n    }",
    "1-0": "to.FBID",
    "2-0": "to.GPID",
    "3-0": "to.Email",
    "4-0": "to.Identity",
    "5-0": "to.objectId",
    "6-0": "tag_group",
    "7-0": "when",
    "8-0": "content",
    "9-0": "content.title",
    "10-0": "content.body",
    "11-0": "content.subject",
    "12-0": "content.sender_name",
    "14-0": "notification_sent",
    "15-0": "respect_frequency_caps",
    "16-0": "estimate_only",
    "16-1": "If this parameter is set to true the request will return an estimated reach of the campaign, which is the number of users who will get the notification when you send it out. Setting this parameter to true will not create the campaign.",
    "16-2": "optional",
    "16-3": "boolean",
    "16-4": "false",
    "15-2": "optional",
    "15-3": "boolean",
    "15-4": "false",
    "15-1": "Set to false if you want to override frequency caps.",
    "14-2": "optional",
    "12-1": "Sender name for your email which is sent to the user.",
    "12-2": "required",
    "12-3": "string",
    "12-4": "“Onboarding”",
    "11-1": "Subject content of your email which is sent to the user.",
    "11-2": "required",
    "11-3": "string",
    "11-4": "\"Subject\"",
    "10-1": "Body content of your push notification, web push, email and sms which is sent to the user.",
    "9-1": "Title content of your push notification message and web push message which is sent to the user.",
    "9-2": "required",
    "10-2": "required",
    "9-3": "string",
    "10-3": "string",
    "10-4": "“Have you seen the special offer?”",
    "9-4": "\"Hi\"",
    "7-1": "When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.",
    "8-1": "Object that defines the content for your message.",
    "8-2": "required",
    "7-2": "required",
    "8-4": "\"content\": {\n        \"subject\": \"Welcome\",\n        \"body\": \"<div>Your HTML content for the email</div>\",\n        \"sender_name\": \"CleverTap\"\n    }",
    "8-3": "object",
    "7-3": "string",
    "7-4": "“now”",
    "1-1": "Facebook ID for user.",
    "2-1": "Google ID for user.",
    "3-1": "Email identifier for user.",
    "4-1": "Custom-defined identifier for user.",
    "5-1": "CleverTap identifier for user.",
    "6-1": "Each message can be tagged into a tag group. Metrics, such as sent and viewed, will be shown against each tag group in the CleverTap dashboard. Max 20 groups. If not specified, the merics will be shown against the “Not Tagged” tag.",
    "1-2": "required",
    "2-2": "required",
    "3-2": "required",
    "4-2": "required",
    "5-2": "required",
    "6-2": "required",
    "6-4": "\"my tag group\"",
    "6-3": "string",
    "1-3": "array of strings",
    "2-3": "array of strings",
    "3-3": "array of strings",
    "4-3": "array of strings",
    "5-3": "array of strings",
    "1-4": "\"to\": {\n        \"FBID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ]\n}",
    "2-4": "{\n    \"to\": {\n        \"GPID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ]\n}",
    "3-4": "{\n    \"to\": {\n        \"Email\": [\n            \"john@doe.com\",\n            \"jan@doe.com\"\n        ]\n}",
    "4-4": "{\n    \"to\": {\n      \"Identity\": [\n            \"JohnDoe\"\n        ]\n}",
    "5-4": "{\n    \"to\": {\n        \"objectId\": [\n \"sdnkansdjknaskdjnasjkndja\",     \"dffajjdfoaiaefiohnefwprjf\"\n        ]\n}",
    "13-0": "provider_nick_name",
    "13-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter lets you select which provider to use for the campaign. For example, let's say you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid.",
    "13-2": "optional",
    "13-3": "string",
    "13-4": "\"Provider 1\""
  },
  "cols": 5,
  "rows": 17
}
[/block]
Below are example payloads for web push, push, email, and SMS.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"to\": {\n        \"FBID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ],\n        \"GPID\": [\n            \"1928288389299292\"\n        ],\n        \"Email\": [\n            \"john@doe.com\",\n            \"jane@doe.com\"\n        ],\n        \"Identity\": [\n            \"JohnDoe\"\n        ],\n        \"objectId\": [\n            \"_asdnkansdjknaskdjnasjkndja\",\n            \"-adffajjdfoaiaefiohnefwprjf\"\n        ]\n    },\n    \"tag_group\": \"my tag group\",\n    \"respect_frequency_caps\": false,\n     \"content\": {\n        \"title\": \"Hi!\",\n        \"body\": \"How are you doing today?\",\n        \"platform_specific\": {\n            \"chrome\": {\n                \"image\": \"https://www.exampleImage.com\",\n              \"icon\":\"https://www.exampleIcon.com\",\n                \"deep_link\": \"http://www.example.com\",\n              \"ttl\":10,\n              \"require_interaction\":true,\n              \"cta_title1\":\"title\",\n              \"cta_link1\":\"http://www.example2.com\",\n              \"cta_iconlink1\":\"https://www.exampleIcon2.com\"\n            }\n        }\n    }\n}",
      "language": "json",
      "name": "Web Push"
    },
    {
      "code": "{\n    \"to\": {\n        \"FBID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ],\n        \"GPID\": [\n            \"1928288389299292\"\n        ],\n        \"Email\": [\n            \"john@doe.com\",\n            \"jane@doe.com\"\n        ],\n        \"Identity\": [\n            \"JohnDoe\"\n        ],\n        \"objectId\": [\n            \"_asdnkansdjknaskdjnasjkndja\",\n            \"-adffajjdfoaiaefiohnefwprjf\"\n        ]\n    },\n    \"tag_group\": \"my tag group\",\n    \"respect_frequency_caps\": false,\n    \"content\": {\n        \"title\": \"Welcome\",\n        \"body\": \"Hello world!\",\n        \"platform_specific\": {\n            \"windows\": {\n                \"deep_link\": \"example.com\",\n                \"key\": \"value_windows\"\n            },\n            \"ios\": {\n                \"deep_link\": \"example.com\",\n                \"sound_file\": \"example.caf\",\n                \"category\": \"notification category\",\n                \"badge_count\": 1,\n                \"key\": \"value_ios\"\n            },\n            \"android\": {\n                \"background_image\": \"http://example.jpg\",\n                \"default_sound\": true,\n                \"deep_link\": \"example.com\",\n                \"large_icon\": \"http://example.png\",\n                \"key\": \"value_android\"\n            }\n        }\n    }\n}",
      "language": "json",
      "name": "Push"
    },
    {
      "code": "{\n    \"to\": {\n        \"FBID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ],\n        \"GPID\": [\n            \"1928288389299292\"\n        ],\n        \"Email\": [\n            \"john@doe.com\",\n            \"jane@doe.com\"\n        ],\n        \"Identity\": [\n            \"JohnDoe\"\n        ],\n        \"objectId\": [\n            \"_asdnkansdjknaskdjnasjkndja\",\n            \"-adffajjdfoaiaefiohnefwprjf\"\n        ]\n    },\n    \"tag_group\": \"my tag group\",\n    \"respect_frequency_caps\": false,\n    \"content\": {\n        \"subject\": \"Welcome\",\n        \"body\": \"<div>Your HTML content for the email</div>\",\n        \"sender_name\": \"CleverTap\"\n    }\n}",
      "language": "json",
      "name": "Email"
    },
    {
      "code": "{\n    \"to\": {\n        \"FBID\": [\n            \"102029292929388\",\n            \"114342342453463\"\n        ],\n        \"GPID\": [\n            \"1928288389299292\"\n        ],\n        \"Email\": [\n            \"john@doe.com\",\n            \"jane@doe.com\"\n        ],\n        \"Identity\": [\n            \"JohnDoe\"\n        ],\n        \"objectId\": [\n            \"_asdnkansdjknaskdjnasjkndja\",\n            \"-adffajjdfoaiaefiohnefwprjf\"\n        ]\n    },\n    \"tag_group\": \"my tag group\",\n    \"respect_frequency_caps\": false,\n     \"content\": {\n        \"body\": \"Sms body\"\n    }\n}",
      "language": "json",
      "name": "SMS"
    }
  ]
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"to\":{\"FBID\":[\"102029292929388\",\"114342342453463\"],\"GPID\":[\"1928288389299292\"],\"Email\":[\"john@doe.com\",\"jane@doe.com\"],\"Identity\":[\"JohnDoe\"],\"objectId\":[\"_asdnkansdjknaskdjnasjkndja\",\"-adffajjdfoaiaefiohnefwprjf\"]},\"tag_group\":\"mytaggroup\",\"respect_frequency_caps\":false,\"content\":{\"body\":\"Smsbody\"}}' \\\n\"https://api.clevertap.com/1/targets/sms.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n    \"status\": \"success\",\n    \"message\": \"Added to queue for processing\"\n}",
      "language": "json"
    }
  ]
}
[/block]
A success response implies payload validity and addition to the processing queue. Message delivery is handled by the provider (push or email) and depends on if you provided a valid recipient id.

## Notes
If there are errors, you will receive a response in the format below. 
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"fail\",\n    \"error\": \"<error message here>\",\n    \"code\": <error code here>\n}",
      "language": "json"
    }
  ]
}
[/block]
Here are the possible error codes.
21 – to is a mandatory field
84 – Invalid recipients”
73 – “respect_frequency_caps” must be a Boolean
78 – Invalid JSON payload
89 – Unexpected error, please try again

# Unsubscribe Link for Emails 
If you’re using Amazon SES or your own SMTP gateway, you’ll have to follow the steps [listed on this page](https://docs.clevertap.com/docs/handling-unsubscribes) to enable your users to unsubscribe from your email notifications.

# Next Steps
Now that you’ve learned how send a notification from the CleverTap API, learn how to get metrics about a campaign using the [Get Campaign Report API](doc:get-campaign-report-api).
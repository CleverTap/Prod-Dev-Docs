---
title: Webhooks
excerpt: ''
deprecated: false
hidden: false
link:
  new_tab: false
  url: https://docs.clevertap.com/docs/webhooks-overview
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

CleverTap webhooks enable developers to monitor specific user events and receive push notifications to their server when those events happen. CleverTap webhooks are often used to trigger business workflows. For example, if you want to get notified every time a user looks up an international flight in business class but then doesn’t book the flight within 30 minutes. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a process in your call center to contact the user to complete the booking.

Another popular use case for CleverTap webhooks is with e-commerce companies and cart abandonment. For example, if a user abandons their cart during a high-value transaction. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a call center workflow to contact the customer. 

There are six categories of webhooks that you can create to monitor user events. The table below describes each of those categories.
[block:parameters]
{
  "data": {
    "h-0": "Type",
    "h-1": "Description",
    "h-2": "Example",
    "0-0": "Single action",
    "1-0": "Actions",
    "2-0": "Inaction within time",
    "3-0": "Inaction",
    "4-0": "Date time property",
    "5-0": "Actions with user properties",
    "5-1": "Performed an action or not, filtered by their demographic properties.",
    "4-1": "Period of time after a date time property on a user event.",
    "3-1": "Users performed an action, but did not perform another action.",
    "2-1": "User performed an action, but does not perform another action within a certain time.",
    "1-1": "User performed multiple actions.",
    "0-1": "User performed an action.",
    "0-2": "User launched the app.",
    "1-2": "User launched the app and viewed a notification.",
    "2-2": "User has not launched the app in three days.",
    "3-2": "User launched the app, but did not view a notification.",
    "4-2": "User purchased an item and three days have passed.",
    "5-2": "User purchased an item and lives in Canada."
  },
  "cols": 3,
  "rows": 6
}
[/block]
Webhooks are created in the CleverTap dashboard by defining a user segment and a callback URL on your server. CleverTap sends a webhook as an HTTP POST with information about the event when it occurs. You can customize the webhooks to use basic authentication and include any custom properties if needed. 

# Set Up Webhook in CleverTap Dashboard
To set up your first CleverTap webhook, perform the following steps:

1. Navigate to *Settings* > *Engage* > *Channels* > *Webhooks*.
2. Click **+ Add Webhook**. The *Create webhook template* popup opens.
3. Enter the details in the webhook template and click **Save**.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/37b84e2-Create_webhook_template.png",
        "Create webhook template.png",
        1262,
        1836,
        "#f6f7fa"
      ],
      "sizing": "80",
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "HTTP Methods",
  "body": "The following four HTTP methods are available for webhooks templates:\n* POST\n* GET\n* DELETE \n* PUT"
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "Add a webhook name and destination URL. If you currently do not have an endpoint created on your server to receive the webhook, you can use https://requestcatcher.com to create an endpoint for testing purposes.",
  "title": "Testing Considerations"
}
[/block]
# Create a Webhook Campaign
To create a webhook campaign, perform the following steps:

1. Navigate to *Campaigns* from the CleverTap dashboard. 
2. Click **+ Campaign**. The channels page displays. 
3. Click **Webhooks** under *Server* channels. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ba80bca-Campaign_Webhooks_channel_select.png",
        "Campaign_Webhooks_channel_select.png",
        850,
        513,
        "#f8f8fa"
      ],
      "border": true,
      "caption": "",
      "sizing": "full"
    }
  ]
}
[/block]
4. Select the campaign type. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1a570eb-Campaign_Webhooks_campaign_type.png",
        "Campaign_Webhooks_campaign_type.png",
        1066,
        605,
        "#f7f8fa"
      ],
      "border": true
    }
  ]
}
[/block]
5. Select when to send the campaign. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/14998f1-Campaign_Webhooks_campaign_When.png",
        "Campaign_Webhooks_campaign_When.png",
        1106,
        617,
        "#f8f9fc"
      ],
      "sizing": "full",
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Recurring Day",
  "body": "If you specify a recurring day for a campaign, such as the 7th of each month, then the campaign will start for the specified day and ignore the creation date. This is a precaution to avoid sending a campaign unintentionally on a prior date."
}
[/block]

6. If you have an existing segment you want to use for the webhook, select that segment. Otherwise, click **+ Create an ad-hoc segment**. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9e9200a-Campaign_Webhooks_campaign_Segments.png",
        "Campaign_Webhooks_campaign_Segments.png",
        1066,
        524,
        "#f4f5fa"
      ],
      "border": true,
      "caption": ""
    }
  ]
}
[/block]
7. In the *Single action box*, click **+ Create an ad-hoc segment**, and then enter details and click **Continue**.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0172d69-Campaign_Webhooks_campaign_What.png",
        "Campaign_Webhooks_campaign_What.png",
        1110,
        618,
        "#f8f9fc"
      ],
      "border": true,
      "caption": ""
    }
  ]
}
[/block]

 8. Select *webhook URL* from the list. 
 9. Add query parameters in the URL key-value pairs section. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bec6e34-Campaign_Webhooks_Message_compose_main.png",
        "Campaign_Webhooks_Message_compose_main.png",
        673,
        569,
        "#f9f9fb"
      ],
      "sizing": "full",
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "You can also click the **Add a new webhook URL** to create a new Webhook URL. You will be redirected to the settings page. Refer to [Set up a Webhook](https://developer.clevertap.com/docs/webhooks#section-step-1-set-up-webhook-in-clever-tap-dashboard)."
}
[/block]
10. Select the content type (JSON or plain text).
11. Add the webhook content to send or receive the data. 

You can either use *Profile Variables* or *Custom* body.  You can also add personalization with the @ symbol. You have the option to send/receive an email, identity, objectId (CleverTap ID), profileData (custom profile variables), and push_token for each user.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2fc7d82-Campaign_Webhooks_Message_compose_profile_variables_zoom.png",
        "Campaign_Webhooks_Message_compose_profile_variables_zoom.png",
        711,
        895,
        "#f4f6f8"
      ],
      "sizing": "full",
      "border": true
    }
  ]
}
[/block]
You can also choose a custom payload. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7cfff5b-Campaign_Webhooks_Message_compose_custom_payload_zoom.png",
        "Campaign_Webhooks_Message_compose_custom_payload_zoom.png",
        685,
        794,
        "#f4f6f8"
      ],
      "border": true,
      "sizing": "full"
    }
  ]
}
[/block]
The preview for your payload is displayed on the right side of the pane. The final view will look similar to the following:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/72e99b7-Campaign_Webhooks_Preview.png",
        "Campaign_Webhooks_Preview.png",
        1353,
        1299,
        "#f9fafb"
      ],
      "border": true,
      "sizing": "full"
    }
  ]
}
[/block]

###Liquid Tags

Liquid tags offer flexibility in composing your message. You can have fixed or variable values and change the look and feel of your message by using Liquid tags. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ed642bf-campaigns_liquid_tags_common_example.png",
        "campaigns_liquid_tags_common_example.png",
        1357,
        470,
        "#fafafb"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]
Each notification is personalized to the receiver. 
[block:code]
{
  "codes": [
    {
      "code": "Hello John!\n\nHere is a special coupon for you: Gold4All\n",
      "language": "html",
      "name": "Output - Liquid"
    }
  ]
}
[/block]
For more information on using tags, refer to [Liquid Tags](https://docs.clevertap.com/docs/liquid-tags#using-tags).

### Linked Content

Linked content gives you the ability to personalize messages with rich and contextual data that is available outside of the CleverTap platform. Linked Content helps you to send messages with send-time personalization.

For example, you deliver food across different geographies and you want to personalize offers to each user based on the weather on their location. The location can come from CleverTap personalization and the weather information can come from an API that provides the current weather. If you want to add more information, you can opt for any source such as third-party tools and in-house software to include in your message.

For more information on using Linked Content, refer to [Linked Content](https://docs.clevertap.com/docs/linked-content#write-linked-content).

To use linked content, perform the following steps:
1. Click the **send a test webhook** link to test if your webhook is set up and working correctly. 
2. Click **Continue**. Check the *Overview* and deploy the webhook. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/07443ab-Campaign_Webhooks_campaign_Overview.png",
        "Campaign_Webhooks_campaign_Overview.png",
        1069,
        1069,
        "#f8f9fb"
      ],
      "sizing": "full",
      "border": true
    }
  ]
}
[/block]

# Listen for Webhooks on Your Server
In the previous step, we created a webhook to send notifications from our server when a user makes a purchase. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c361c62-image11.png",
        "image11.png",
        738,
        87,
        "#fafafa"
      ],
      "border": true,
      "caption": ""
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Understanding Webhook Errors",
  "body": "Webhook is a request-response type format. CleverTap sends the request and waits for three seconds for a response from the endpoint. If the endpoint URL fails to respond then CleverTap retries the request. If the retry also fails, the campaign stats page is updated with the error \"Webhook Dispatch Failed.\" \n\nAlso, another probable cause for the error is if the webhook endpoint is temporarily unavailable or down."
}
[/block]

## Response Formats
The following are the two types of response formats sent by CleverTap when creating a webhook campaign:

### Broadcast Message:
The message is termed as Broadcast when the following four conditions are met:
  * User selects *JSON* from the *Webhook content format* dropdown
  * User selects *Profile variables& custom key value pairs* as the message body
  *  Webhook URL does not include any personalization component
  * Key-value pairs do not include any liquid tags. However, the message still qualifies as a broadcast message if the key-value pairs use *@Personalization*.

For broadcast messages, the payload includes an array of profile variables. The following is a sample webhook payload containing all possible fields for profile variables. The `targetId` corresponds to the webhook campaign ID for which you receive the payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"targetId\": 1472124953,\n    \"key_values\": {\n        \"PromoCode\": \"MT50\",\n        \"PowerUser\": \"true\"\n    },\n    \"profiles\": [\n        {\n            \"Email\": \"jack@gmail.com\",\n            \"Identity\": \"foo\",\n            \"ObjectId\": \"-g55b74fb1030740e4a4931910a8abb862\",\n            \"ProfileData\": {\n                \"Last Score\": 308,\n                \"High Score\": 308,\n                \"Replayed\": true\n            },\n            \"Event_properties\": {\n                \"Score\": 1200,\n                \"Game Mode\": \"Practice\",\n                \"Rating\": 1\n            },\n            \"Push_token\": \"19403aa5d3bb376769a8101c9cbf22159cb1836117659cc684a71243589982ea\"\n        },\n        {\n            \"Email\": \"jill@gmail.com\",\n            \"Identity\": \"bar\",\n            \"ObjectId\": \"__g09c9bf3b0d374a259c86f5b855ec9b19\",\n            \"ProfileData\": {\n                \"Last Score\": 309,\n                \"High Score\": 309,\n                \"Replayed\": true\n            },\n            \"Event_properties\": {\n                \"Score\": 500,\n                \"Game Mode\": \"Tournament\",\n                \"Rating\": 2\n            },\n            \"Push_token\": \"fiCjRO_opjw:APA91bH0yzS_eBj1Xx8TcakMzz4yTc3id29A79GOjnaaMtIQLrjztNfzzRz7slqWmNzndECx_hHh7qiL0LZJOkGwzr9Zp_g1mC9B6BVuEbsjrMFoxXX830zMRuvxtU_AhFTC0JPHejZh\"\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
### Non-broadcast Message
The messages other than broadcast messages are termed Non-broadcast messages. The following is a sample webhook payload containing all possible fields for profile variables. The `targetId` corresponds to the webhook campaign ID for which you are receiving the payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"targetId\": 1472124953,\n    \"key_values\": {\n        \"PromoCode\": \"MT50\",\n        \"PowerUser\": \"true\"\n    },\n    \"profiles\":\n        {\n            \"Email\": \"jack@gmail.com\",\n            \"Identity\": \"foo\",\n            \"ObjectId\": \"-g55b74fb1030740e4a4931910a8abb862\",\n            \"ProfileData\": {\n                \"Last Score\": 308,\n                \"High Score\": 308,\n                \"Replayed\": true\n            },\n            \"Event_properties\": {\n                \"Score\": 1200,\n                \"Game Mode\": \"Practice\",\n                \"Rating\": 1\n            },\n            \"Push_token\": \"19403aa5d3bb376769a8101c9cbf22159cb1836117659cc684a71243589982ea\"\n        }",
      "language": "json"
    }
  ]
}
[/block]

#View Statistics

You can view the statistics of your webhook from the *Campaign* list page.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8a47af5-Campaign_Webhooks_Statistics.png",
        "Campaign_Webhooks_Statistics.png",
        1381,
        1345,
        "#fafbfb"
      ],
      "sizing": "full",
      "border": true
    }
  ]
}
[/block]
Another option besides setting up an endpoint on your server to listen for CleverTap webhooks is using a workflow automation tool, such as Tray.io or Zapier. These tools can help integrate a CleverTap webhook without any code and then send these notifications to other systems such as Slack.
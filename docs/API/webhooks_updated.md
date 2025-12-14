---
title: Webhooks_Updated
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
CleverTap webhooks enable developers to monitor specific user events and receive push notifications to their server when those events happen.

# Overview

CleverTap webhooks are often used to trigger business workflows. For example, let’s say you want to get notified every time a user looks up an international flight in business class, but then doesn’t book the flight within 30 minutes. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a process in your call center to contact the user to complete the booking.

Another popular use case for CleverTap webhooks is with ecommerce companies and cart abandonment. Let’s say a user abandons their cart during a high-value transaction. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a call center workflow to contact the user to solve any issues and complete the purchase. 

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
Webhooks are created in the CleverTap dashboard by defining a user segment and a callback URL on your server. CleverTap will send a webhook as a HTTP POST with information about the event when it occurs. You can customize the webhooks to use basic authentication and include any custom properties if needed. 

The rest of this page walks through how to create your first CleverTap webhook. 

# Step 1: Set Up Webhook in CleverTap Dashboard

Go to Settings>  Engage > Channels > Webhooks.
Click the +Add Webhook button. 
Update the webhook template.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c5ed2e7-Campaign_Webhooks_add_webhook_param.png",
        "Campaign_Webhooks_add_webhook_param.png",
        606,
        875,
        "#f6f7f9"
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
  "body": "Add a Webhook name and Destination URL. If you currently don’t have an endpoint created on your server to receive the webhook, you can use www.Requestbin.com to create an endpoint for testing purposes.",
  "title": "Note"
}
[/block]
Click Save.

# Step2: Create a Webhook Campaign

Go to the Campaigns from the CleverTap dashboard. Click the + Campaign button. The channels page appears. Click "Webhooks" under Server channels. 
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
      "caption": "Image replacement",
      "sizing": "full"
    }
  ]
}
[/block]
Select the campaign type. 
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
Select when to send the campaign. 
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
      "sizing": "full"
    }
  ]
}
[/block]
If you have an existing segment that you want use for the webook, select that segment. Otherwise, click the + Create ad-hoc segment button. 
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
      "caption": "Image replacement"
    }
  ]
}
[/block]
In the Single action box, click + Create an ad-hoc segment. Enter details and click Continue.
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
      "caption": "Image replacement"
    }
  ]
}
[/block]

  * Select Webhook URL from the list. 
  * Add query parameters in URL key-value pairs. 



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
      "sizing": "full"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "You can also click the \"Add a new webhook URL\" to create a new Webhook URL. You will be redirected to the settings page. See [Set up Webhook](https://developer.clevertap.com/docs/webhooks_updated#section-step-1-set-up-webhook-in-clever-tap-dashboard)."
}
[/block]
* Select the content-type (JSON or plain text)


* Add the Webhook content to send or receive the data. You can either use Profile Variables or Custom body.  You can also add personalization with "@" here. You have the option to send/receive email, identity, objectId (CleverTap ID), profileData (custom profile variables), and push_token for each user.


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
The preview for your payload is displayed on the right side of the pane. The final view will look something similar:
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
Click the "send a test webhook" link to test if your Webhook is set up and working correctly. 

Apply control groups, if required. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/25b0f14-Campaign_Webhooks_campaign_setup_control_groups.png",
        "Campaign_Webhooks_campaign_setup_control_groups.png",
        1380,
        571,
        "#f7f8fb"
      ]
    }
  ]
}
[/block]
Click Continue. Check the Overview and deploy the Webhook. 
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
# Step 3: Listen for Webhooks on Your Server

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
      "caption": "Image replacement"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Understanding Webhook errors",
  "body": "Webhook is a request-response type format. CleverTap sends the request and waits for one second for a response from the end point. If the end point URL fails to respond within one second, CleverTap retries the request. If the retry also fails, the campaign stats page is updated with the error 'Webhook Dispatch Failed'. \n\nAlso, another probable cause for the error is if the webhook endpoint is temporarily unavailable or down."
}
[/block]

Below is a sample webhook payload containing all possible fields for profile variables. The TargetId corresponds to the webhook campaign ID for which you are receiving the payload. 
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
#View Statistics

You can view the statistics of your Webhook from the Campaigns list page.
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
Congratulations on creating your first CleverTap webhook!

Another option besides setting up an endpoint on your server to listen for CleverTap webhooks is using a workflow automation tool, such as Tray.io or Zapier. These tools can help integrate a CleverTap webhook without any code and then send these notifications to other systems such as Slack. If you are interested in learning more, we have [a guide](https://docs.clevertap.com/docs/send-abandoned-cart-users-to-slack) that walks through sending a CleverTap webhook to Slack via Tray.io.
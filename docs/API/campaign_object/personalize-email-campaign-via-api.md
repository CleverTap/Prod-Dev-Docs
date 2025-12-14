---
title: Create Campaign
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The Create Campaign API lets you send messages to your users. You can specify the audience for your message in two ways. The first option is to target people based on user events and properties. The second option is to target people based on their user ids.

For example, you can use this API to send a price drop alert as a [push notification](https://docs.clevertap.com/docs/push) to users who have viewed a certain product in the past week. If you have thousands of products and hundreds of thousands of users, this API lets you create a scalable and targeted way to notify your users of price drop alerts.

For more information on API request limit, refer to [API Request Limit](doc:api-request-limit).

> 📘 Note
> 
> Currently, AMP Email campaigns cannot be created using Create Campaign API.

# Create Campaign API - Target User Events & Properties

To create a campaign, you have to specify the message, channel, and people to target by their user events and properties. For example, a user event could be a purchase event and the user property could be their location.

This endpoint is limited to 3 requests per second.

## Base URL - Target User Events & Properties

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/targets/create.json>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method - Target User Events & Properties

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

> 📘 Note
> 
> CleverTap supports Liquid personalization for Push Campaigns sent via Create Campaign API. For more information refer [Liquid Personalization in Campaign APIs](https://docs.clevertap.com/docs/personalize-messages-push#liquid-personalization-in-campaign-apis).

## Body Parameters - Target User Events & Properties

> 📘 Mobile Push Notification Parameters
> 
> The parameters in bold and marked with \* provide additional functionality for creating a push campaign.

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required/Optional",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "name",
    "0-1": "The name of your campaign shown in the CleverTap dashboard.",
    "0-2": "Required",
    "0-3": "String",
    "0-4": "“My Campaign”",
    "1-0": "target_mode",
    "1-1": "Allows you to specify the type of campaign being created, such as push notifications, web push, email, etc.",
    "1-2": "Required",
    "1-3": "String",
    "1-4": "\"Push\"",
    "2-0": "provider_nick_name",
    "2-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is only for Email, WhatsApp, and SMS.",
    "2-2": "Required",
    "2-3": "String",
    "2-4": "\"Provider 1\"",
    "3-0": "provider_group_nickname",
    "3-1": "Allows you to specify which provider group to use for your SMS campaign. You must use the name as added in your account settings",
    "3-2": "Optional",
    "3-3": "String",
    "3-4": "\"NewTestSMSProviderGroup\"",
    "4-0": "content.sms.template_id",
    "4-1": "Template ID (required for sending SMS to phone numbers of India)",
    "4-2": "Optional",
    "4-3": "String",
    "4-4": "",
    "5-0": "**conversion_goal\\***",
    "5-1": "Checks the end goal of the campaign for conversion tracking.",
    "5-2": "Optional",
    "5-3": "Object",
    "5-4": "\"conversion_goal\": {  \n  \"event_name\": \"Charged\",  \n  \"filter_type\": {  \n    \"event_property\": \\[  \n      {  \n        \"property_name\": \"Items|Book name\",  \n        \"operator\": \"equals\",  \n        \"property_value\": \"book name\"  \n      },  \n      {  \n        \"property_name\": \"Items|Category\",  \n        \"operator\": \"equals\",  \n        \"property_value\": \"category\"  \n      },  \n      {  \n        \"property_name\": \"Payment mode\",  \n        \"operator\": \"equals\",  \n        \"property_value\": \"UPI\"  \n      },  \n      {  \n        \"property_name\": \"Amount\",  \n        \"operator\": \"between\",  \n        \"property_value\": [3456, 4567]  \n      }  \n    ],  \n    \"first_time\": \"yes\",  \n    \"last_time\": \"yes\",  \n    \"time_of_day\": \\[[\"21:00\", \"23:00\"]],  \n    \"day_of_week\": [1, 3],  \n    \"day_of_month\": [1, 3, 16, 31]  \n  },  \n  \"conversion_time\": \"3D\",  \n  \"revenue_property\": \"Amount\"  \n}",
    "6-0": "**conversion_goal.event_name\\***",
    "6-1": "Name of the goal you want to track.",
    "6-2": "Required if conversion_goal is provided",
    "6-3": "String",
    "6-4": "\"event_name\": \"Charged\",",
    "7-0": "**conversion_goal.filter_type\\***",
    "7-1": "Filters for provided event_name  \nThe type of filter applied in conversion tracking. For example, first_time, time_of_day and so on.",
    "7-2": "Required if conversion_goal is provided",
    "7-3": "Object",
    "7-4": "\"filter_type\": {  \n  \"event_property\": [  \n    {  \n      \"property_name\": \"Items|Book name\",  \n      \"operator\": \"equals\",  \n      \"property_value\": \"book name\"  \n    }  \n  ],  \n  \"first_time\": \"yes\",  \n  \"last_time\": \"yes\",  \n  \"time_of_day\": [\"21:00\", \"23:00\"],  \n  \"day_of_week\": [1, 3],  \n  \"day_of_month\": [1, 3, 16, 31]  \n}",
    "8-0": "**conversion_goal. filter_type.  \nevent_property\\***",
    "8-1": "Event Property for given event_name.",
    "8-2": "Optional",
    "8-3": "List",
    "8-4": "\"event_property\": \\[  \n  {  \n    \"property_name\": \"Items|Book name\",  \n    \"operator\": \"equals\",  \n    \"property_value\": \"book name\"  \n  },  \n  {  \n    \"property_name\": \"Items|Category\",  \n    \"operator\": \"equals\",  \n    \"property_value\": \"category\"  \n  },  \n  {  \n    \"property_name\": \"Payment mode\",  \n    \"operator\": \"equals\",  \n    \"property_value\": \"UPI\"  \n  },  \n  {  \n    \"property_name\": \"Amount\",  \n    \"operator\": \"between\",  \n    \"property_value\": [3456, 4567]  \n  }  \n]",
    "9-0": "**conversion_goal.filter_type.  \nevent_property.property_name\\***",
    "9-1": "Name of the event_property.",
    "9-2": "Optional",
    "9-3": "String",
    "9-4": "\"property_name\": \"Items|Book name\"",
    "10-0": "**conversion_goal.filter_type.  \nevent_property.operator\\***",
    "10-1": "Operator required for property_name and property_value.  \nValid values are:  \nequals  \ngreater_than  \ngreater_than_equals  \nless_than  \nless_than_equals  \ncontains  \ndoes_not_contain  \nnot_equals  \nexists (does not require property_value)  \ndoes_not_exist (does not require property_value)  \nbetween (requires 2 values in an array)",
    "10-2": "Optional",
    "10-3": "String",
    "10-4": "\"operator\":\"equals\",",
    "11-0": "**conversion_goal.filter_type.  \nevent_property.property_value\\***",
    "11-1": "Provides value of the event_property.",
    "11-2": "Optional",
    "11-3": "String",
    "11-4": "\"property_value\":\"book name\"",
    "12-0": "**conversion_goal.  \nfilter_type.first_time\\***",
    "12-1": "Provides value of the first_time.  \nValid values are:  \n\"yes\"  \n\"no\"",
    "12-2": "Optional",
    "12-3": "String",
    "12-4": "\"first_time\":\"yes\",",
    "13-0": "**conversion_goal.  \nfilter_type.last_time\\***",
    "13-1": "Provides value of last_time.  \nValid values:  \n\"yes\"  \n\"no\"",
    "13-2": "Optional",
    "13-3": "String",
    "13-4": "\"last_time\":\"yes\",",
    "14-0": "**conversion_goal.  \nfilter_type.time_of_day\\***",
    "14-1": "Provides value of time_of_day.  \nValid values:  \n\\[ [from_time, to_time] , [from_time, to_time] ]  \ntime: \"HH:MM\"",
    "14-2": "Optional",
    "14-3": "List of lists",
    "14-4": "\"time_of_day\": \\[  \n  [\"21:00\", \"23:00\"],  \n  [\"11:34\", \"12:44\"],  \n  [\"13:01\", \"13:40\"]  \n]",
    "15-0": "**conversion_goal.  \nfilter_type.day_of_week\\***",
    "15-1": "Provides value of day_of_week.  \nValid values:  \n[0 to 6] is [Sun to Sat]",
    "15-2": "Optional",
    "15-3": "List",
    "15-4": "\"day_of_week\": [1, 3, 5]",
    "16-0": "**conversion_goal.  \nfilter_type.day_of_month\\***",
    "16-1": "Provides value of day_of_month.  \nValid values:  \n[dates_of_month,....]  \nrange: [1 to 31]",
    "16-2": "Optional",
    "16-3": "List",
    "16-4": "\"day_of_month\": [1, 3, 16, 31]",
    "17-0": "**conversion_goal.conversion_time\\***",
    "17-1": "Denotes the period that conversions are tracked for once the campaign is published.  \nAccepted values:  \n[30m, 1H, 2H, 4H, 8H, 12H, 1D, 2D, 3D, 5D, 1W, 2W, 1M, 2M, 5M]  \nwhere, m: minutes, H: hour, D: day, W: week, M:month",
    "17-2": "Required if conversion_goal is provided",
    "17-3": "String",
    "17-4": "\"conversion_goal\": {  \n  \"event_name\": \"Charged\",  \n  \"filter_type\": {  \n    \"event_property\": \\[  \n      {  \n        \"property_name\": \"Items|Book name\",  \n        \"operator\": \"equals\",  \n        \"property_value\": \"book name\"  \n      },  \n      {  \n        \"property_name\": \"Items|Category\",  \n        \"operator\": \"equals\",  \n        \"property_value\": \"category\"  \n      },  \n      {  \n        \"property_name\": \"Payment mode\",  \n        \"operator\": \"equals\",  \n        \"property_value\": \"UPI\"  \n      },  \n      {  \n        \"property_name\": \"Amount\",  \n        \"operator\": \"between\",  \n        \"property_value\": [3456, 4567]  \n      }  \n    ],  \n    \"first_time\": \"yes\",  \n    \"last_time\": \"yes\",  \n    \"time_of_day\": \\[[\"21:00\", \"23:00\"]],  \n    \"day_of_week\": [1, 3],  \n    \"day_of_month\": [1, 3, 16, 31]  \n  },  \n  \"conversion_time\": \"3D\",  \n  \"revenue_property\": \"Amount\"  \n}",
    "18-0": "**conversion_goal.revenue_property\\***",
    "18-1": "Denotes the revenue property for conversion goal event. For example, amount, currency, payment mode and so on.",
    "18-2": "Required if conversion_goal is provided",
    "18-3": "String",
    "18-4": "\"revenue_property\": \"Amount\"",
    "19-0": "where",
    "19-1": "Allows you to filter target base on the user events and profile properties. Send an empty object ({}) to target your entire user base.",
    "19-2": "Required",
    "19-3": "Object",
    "19-4": "\"where\": {  \n  \"event_name\": \"Charged\",  \n  \"from\": 20251001,  \n  \"to\": 20251220,  \n  \"common_profile_properties\": {  \n    \"profile_fields\": [  \n      {  \n        \"name\": \"Customer Type\",  \n        \"operator\": \"equals\",  \n        \"value\": \"Platinum\"  \n      }  \n    ]  \n  }  \n}",
    "20-0": "where.event_name",
    "20-1": "Allows you to target users based on an event they perform.",
    "20-2": "Optional",
    "20-3": "String",
    "20-4": "\"Charged\"",
    "21-0": "where.from",
    "21-1": "Start of date range for event needed. Value specified in format YYYYMMDD.",
    "21-2": "Optional",
    "21-3": "Int",
    "21-4": "20251001",
    "22-0": "where.to",
    "22-1": "End of date range for event needed. Value specified in format YYYYMMDD.",
    "22-2": "Optional",
    "22-3": "Int",
    "22-4": "20251220",
    "23-0": "where.profile_fields",
    "23-1": "Allows you to target users based on the values of their profile fields.",
    "23-2": "Optional",
    "23-3": "Int",
    "23-4": "\"common_profile_properties\": {  \n  \"profile_fields\": [  \n    {  \n      \"name\": \"Customer Type\",  \n      \"operator\": \"equals\",  \n      \"value\": \"Platinum\"  \n    }  \n  ]  \n}",
    "24-0": "estimate_only",
    "24-1": "If this parameter is set to true, the request returns an estimated reach of the campaign, which is the number of users who receive the notification when you send it out. Setting this parameter to true does not create the campaign.",
    "24-2": "Optional",
    "24-3": "Boolean",
    "24-4": "FALSE",
    "25-0": "async_estimate",
    "25-1": "This allows setting of the estimate_only flag asynchronously. If this parameter is set to true, it returns a request ID that can be passed in the subsequent API call.",
    "25-2": "Optional",
    "25-3": "Boolean",
    "25-4": "Boolean",
    "26-0": "system_control_group_include",
    "26-1": "If the system control group is enabled from the dashboard, you can choose to use the control group for API campaigns.",
    "26-2": "Optional",
    "26-3": "Boolean",
    "26-4": "",
    "27-0": "control_group",
    "27-1": "If the custom control group is enabled from the dashboard, you can choose to use the control group for API campaigns",
    "27-2": "Optional",
    "27-3": "String",
    "27-4": "\"control_group\": {  \n\"type\": \"custom\"  \n\"name\": \"name of control group\"  \n}",
    "28-0": "control_group",
    "28-1": "You can create a campaign control group via API by specifying the % as described on the left",
    "28-2": "Optional",
    "28-3": "Integer",
    "28-4": "\"control_group\": {  \n\"type\": \"campaign\"  \n\"percentage\": 5  \n}",
    "29-0": "skip_estimate",
    "29-1": "Toggle this flag to true if you do not want to estimate the reach. This will lead to faster campaign delivery.",
    "29-2": "Optional",
    "29-3": "Boolean",
    "29-4": "\"skip_estimate\":true",
    "30-0": "send_to_all_devices",
    "30-1": "The flag is set to false by default. If no value is specified, then the notification is sent to the last active device. Toggle this flag to true if you want to send notification to all active devices.",
    "30-2": "Optional",
    "30-3": "Boolean",
    "30-4": "FALSE",
    "31-0": "segment",
    "31-1": "The value for this key is the segment ID. The campaign is created only if the segment is processed. If you add this key, you need not mention the \"where\" clause key.",
    "31-2": "Optional",
    "31-3": "Integer",
    "31-4": "upper_cap_for_target_segment\\*",
    "32-0": "**upper_cap_for_target_segment\\***",
    "32-1": "Do not send the campaign if the target segment exceeds the provided number of users.  \nBy default all target segment users are selected.  \nNote: Do not provide this parameter if users_limit_overall is provided. (Minimum value is 100)",
    "32-2": "Optional",
    "32-3": "Integer",
    "32-4": "\"upper_cap_for_target_segment” : 500",
    "33-0": "**users_limit_overall\\***",
    "33-1": "Send only to the provided number of qualified users.  \nBy default all target segment users are selected.  \nNote: Do not provide this parameter if upper_cap_for_target_segment is provided. (Minimum value is 100)",
    "33-2": "Optional",
    "33-3": "Integer",
    "33-4": "“users_limit_overall” : 250",
    "34-0": "**users_limit_per_run\\***",
    "34-1": "Send only to the provided number of users per run.",
    "34-2": "Optional",
    "34-3": "Integer",
    "34-4": "\"users_limit_per_run\":199,",
    "35-0": "send_email_to_opted_out_users",
    "35-1": "Valid values are: true and false. Set to false by default. When true, emails are sent to all qualified users, including those who have unsubscribed. For more information, refer to [Override Communication Preferences for Email](https://docs.clevertap.com/docs/create-message-email#override-communication-preferences-for-email).",
    "35-2": "Optional",
    "35-3": "Boolean",
    "35-4": "\"send_email_to_opted_out_users\":true",
    "36-0": "subscription_groups",
    "36-1": "Campaign is only sent to users who are subscribed to at least one of these groups. For more information, refer to [Manage Email Preferences](https://docs.clevertap.com/docs/manage-email-preferences) .",
    "36-2": "Optional",
    "36-3": "List of Strings",
    "36-4": "",
    "37-0": "content",
    "37-1": "Object that defines the content for your message.",
    "37-2": "Required",
    "37-3": "String",
    "37-4": "\"content\": {  \n\"subject\": \"Welcome\",  \n\"body\": \"  \nYour HTML content for the email  \n\",  \n\"sender_name\": \"CleverTap\"  \n}",
    "38-0": "content.title",
    "38-1": "Title of your push notification message and web push message sent to the user.",
    "38-2": "Required",
    "38-3": "String",
    "38-4": "“Hi”",
    "39-0": "content.body",
    "39-1": "Body content of your push notification, web push, email and sms sent to the user.",
    "39-2": "Required",
    "39-3": "String",
    "39-4": "“Have you seen the special offer?”",
    "40-0": "content.subject",
    "40-1": "Subject of your email sent to the user.",
    "40-2": "Required",
    "40-3": "String",
    "40-4": "“Subject”",
    "41-0": "content.sender_name",
    "41-1": "Sender name for your email sent to the user.",
    "41-2": "Required",
    "41-3": "String",
    "41-4": "“Onboarding”",
    "42-0": "content.template_name",
    "42-1": "Namespace of the WhatsApp template you want to choose. This is available in settings for all the added WhatsApp templates.",
    "42-2": "Required",
    "42-3": "String",
    "42-4": "\"662da022_5583_406d_9c71_428d5e3164bb:ticket_update\"",
    "43-0": "content.replacements",
    "43-1": "Add values that replace the placeholders in the templates.",
    "43-2": "Required",
    "43-3": "String",
    "43-4": "\"replacements\" : {  \n\"1\" : ,  \n\"2\" : ,  \n\"3\" :  \n}",
    "44-0": "content.header",
    "44-1": "Allows you to attach images, videos, documents, and locations in your WhatsApp message based on the attachment type defined in your interactive template.",
    "44-2": "Optional",
    "44-3": "Object",
    "44-4": "",
    "45-0": "content.body",
    "45-1": "Dynamically replaces placeholder values in the body of interactive templates.",
    "45-2": "Required",
    "45-3": "Object",
    "45-4": "",
    "46-0": "content.button",
    "46-1": "Allows you to dynamically add a suffix to CTA buttons with dynamic URL.",
    "46-2": "Optional",
    "46-3": "Object",
    "46-4": "",
    "47-0": "content.locale",
    "47-1": "The language code of the message. To get the language code from the CT dashboard, go to Settings > Channels > Whatsapp and open any template. Click the **Template** tab. The template name displays the locale code in parentheses. For example,  Dynamic URL _(fr)_.",
    "47-2": "Required",
    "47-3": "String",
    "47-4": "\"fr\" for French or \"de\" for German.",
    "48-0": "wzrk_cid",
    "48-1": "Platform-specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.  \nIf a Notification ID is mandatory, then the channel ID is expected. An error occurs if a valid channel ID is not received.  \n  \n**Note:** If the channel ID is not saved on the CleverTap dashboard, then the channel ID does not work in Create Campaign API.",
    "48-2": "Required",
    "48-3": "String",
    "48-4": "",
    "49-0": "wzrk_bc",
    "49-1": "Platform-specific key that can be used when targeting users on devices running Android O - can be used to determine the notification count shown on the app icon",
    "49-2": "Optional",
    "49-3": "Int",
    "49-4": "",
    "50-0": "wqrk_bi",
    "50-1": "Platform-specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.  \nAccepted values:  \napp icon  \nnotification icon",
    "50-2": "Optional",
    "50-3": "String",
    "50-4": "",
    "51-0": "wzrk_acts",
    "51-1": "Android Action Buttons  \n\"l\"= Action Title, \"dl\" = Deep link, \"id\" = Action id, \"ico\" = icon resource identifier",
    "51-2": "Optional",
    "51-3": "Object",
    "51-4": "\"wzrk_acts\": [  \n    {  \n      \"l\": \"Action button 1\",  \n      \"dl\": \"deep link for action button 1\",  \n      \"id\": \"action id of the action button 1\",  \n      \"ico\": \"Icon resource identifier for action button 1\"  \n    }  \n  ]",
    "52-0": "platform_specific",
    "52-1": "The platform-specific details for Push and Web Push notification",
    "52-2": "Optional",
    "52-3": "String",
    "52-4": "",
    "53-0": "enable_rendermax",
    "53-1": "This field helps enable RenderMax when creating a campaign via API. It is available only for Mobile Push campaigns and must be added under the platform_specific key for Android devices only. For more information about the feature, refer to [RenderMax](https://docs.clevertap.com/docs/rendermax). Valid values are: true or false.",
    "53-2": "Optional",
    "53-3": "Boolean",
    "53-4": "\"enable_rendermax\": true",
    "54-0": "background_image",
    "54-1": "The parameter to render images in the push notification for Android.",
    "54-2": "Optional",
    "54-3": "String",
    "54-4": "\"background_image\": \"<http://example.jpg>\"",
    "55-0": "**notification_tray_priority\\***",
    "55-1": "This field helps to set priority for the notification tray in android.  \nValid values are:  \n\"max\"  \n\"high\"  \n\"default\"",
    "55-2": "Optional",
    "55-3": "String",
    "55-4": "\"notification_tray_priority\": \"max\"",
    "56-0": "**delivery_priority\\***",
    "56-1": "This field helps to set delivery priority of notification in android.  \nValid values are:  \n\"normal\"  \n\"high\"",
    "56-2": "Optional",
    "56-3": "String",
    "56-4": "\"delivery_priority\": \"high\"",
    "57-0": "**android_collapse\\***",
    "57-1": "User can send any string value or number to identify collapsible notification in android.",
    "57-2": "Optional",
    "57-3": "String/Integer",
    "57-4": "\"android_collapse\" : \"any message or number\".",
    "58-0": "**ios_collapse\\***",
    "58-1": "User can send any string value or number to identify collapsible notification in iOS.",
    "58-2": "Optional",
    "58-3": "String/Integer",
    "58-4": "\"ios_collapse\" : \"any message or number\"",
    "59-0": "**summary\\***",
    "59-1": "You can provide summary  \nabout the notification you are sending to user valid only in Android.",
    "59-2": "Optional",
    "59-3": "String",
    "59-4": "\"summary\": \"Enter summary here\"",
    "60-0": "**subtitle\\***",
    "60-1": "Subtitle for your notification.",
    "60-2": "Optional",
    "60-3": "String",
    "60-4": "\"subtitle\": \"Enter subtitle\"",
    "61-0": "**large_icon\\***",
    "61-1": "By default, the app icon appears as the large icon. The CleverTap dashboard provides an option to show other icons apart from the app icon.          ",
    "61-2": "Optional",
    "61-3": "String (URL)",
    "61-4": "\"large_icon\": \"logo.com\"",
    "62-0": "**Custom KVs\\***",
    "62-1": "You can specify it in the platform specific object.",
    "62-2": "Optional",
    "62-3": "key-value pair",
    "62-4": "\"platform_specific\": {  \n  \"ios\": {  \n    \"Key\": \"Value_ios\"  \n  },  \n  \"android\": {  \n    \"Key\": \"Value_android\"  \n  }  \n}",
    "63-0": "mutable-content",
    "63-1": "To raise Notification viewed event for Push notifications, raise the flag to \"true\" in the platform_specific: iOS section of the request payload.",
    "63-2": "Optional",
    "63-3": "Boolean",
    "63-4": "",
    "64-0": "respect_frequency_caps",
    "64-1": "Set to false if you want to override frequency caps and dwell time.",
    "64-2": "Optional",
    "64-3": "Boolean",
    "64-4": "TRUE",
    "65-0": "respect_throttle",
    "65-1": "Toggle this flag to true if you want to enable throttle limits for the campaign.",
    "65-2": "Optional",
    "65-3": "Boolean",
    "65-4": "\"respect_throttle\":true",
    "66-0": "ttl",
    "66-1": "This allows setting of time to live for push notifications for android and iOS.  \nThe subkeys for ttl are  \n“ttl_type” - with input as “relative”   \"h-1\":  and ”absolute”  \n“value” -For relative ttl, the input is an integer in minutes.  \nFor absolute ttl, the input should be an integer in the yyyyMMddHHmm format.  \n  \nMore information is available [here](https://docs.clevertap.com/docs/create-message-push).",
    "66-2": "Optional",
    "66-3": "Depends on subkey  \nttl_type: string  \nvalue: Long",
    "66-4": "For Absolute TTL -  \n\"ttl\" : {  \n\"ttl_type\": \"absolute\",  \n\"value\" : 202207200000  \n},  \nFor Relative TTL -  \n\"ttl\" : {  \n\"ttl_type\": \"relative\",  \n\"value\" : 2880  \n},",
    "67-0": "when",
    "67-1": "When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.",
    "67-2": "Required",
    "67-3": "String",
    "67-4": "\"now\"",
    "68-0": "**when\\***",
    "68-1": "Allows you to set Date, time and Delivery preferences of the message.",
    "68-2": "Required",
    "68-3": "Object",
    "68-4": "\"when\": {  \n  \"type\": \"later\",  \n  \"delivery_date_time\": [\"20220420 13:00\", \"20220421 14:00\"],  \n  \"delivery_timezone\": \"user\",  \n  \"user_timezone_wrap_around\": true,  \n  \"dnd\": {  \n    \"message_state\": 1,  \n    \"dnd_timezone\": \"user\",  \n    \"dnd_info\": {  \n      \"2\": [\"21:00\", \"06:00\"],  \n      \"4\": [\"23:56\", \"04:34\"]  \n    }  \n  },  \n  \"campaign_cutoff\": \"14:20\"  \n}",
    "69-0": "**when.type\\***",
    "69-1": "Valid inputs are \"now\" or \"later\" (to send it at a later time) or \"recurring\".",
    "69-2": "Required",
    "69-3": "String",
    "69-4": "\"type\" : \"later\"",
    "70-0": "**when.delivery_date_time\\***",
    "70-1": "\"YYYYMMDD HH:MM\" to schedule the messages for a specific date and time in the future in increasing date order only.",
    "70-2": "Required if when.type = \"later\"",
    "70-3": "String",
    "70-4": "\"delivery_date_time\": [\"20230503 15:20\"]",
    "71-0": "**when.repeats_every\\***",
    "71-1": "Number of days or weeks to be repeated on.",
    "71-2": "Required if when.type = \"recurring\"",
    "71-3": "Integer",
    "71-4": "\"repeats_every\" : 2",
    "72-0": "**when.repeat_type\\***",
    "72-1": "Can be \"day\" or \"week\"  \nNote: Repeat by month is not implemented.",
    "72-2": "Required if when.type = \"recurring\"",
    "72-3": "String",
    "72-4": "\"repeat_type\" : \"week\"",
    "73-0": "**when.start_time\\***",
    "73-1": "\"YYYYMMDD HH:MM\" (Cannot provide past date).",
    "73-2": "Required if when.type = \"recurring\"",
    "73-3": "String",
    "73-4": "\"start_time\" : \"20220415 12:05\",",
    "74-0": "**when.end_by_date\\***",
    "74-1": "\"YYYYMMDD\", End the campaign after provided date.  \nNote: Either end_by_date or end_by_occurrences can be provided.",
    "74-2": "Optional  \nNote: This is applicable only if when.type = \"recurring\"",
    "74-3": "String",
    "74-4": "\"end_by_date\" : \"20220420\"",
    "75-0": "**when.end_by_occurrences\\***",
    "75-1": "End the campaign after provided number of occurrences.  \nNote: Either end_by_date or end_by_occurrences can be provided.",
    "75-2": "Note: This is applicable optional only if when.type = \"recurring\"",
    "75-3": "Integer",
    "75-4": "\"end_by_occurrences\" : 8",
    "76-0": "**when.repeat_on_days_of_week\\***",
    "76-1": "Days of week represented as [1,7] for [Sun to Saturday].",
    "76-2": "Required if when.type = \"recurring\" and repeat_type = \"week\"",
    "76-3": "List",
    "76-4": "\"repeat_on_days_of_week\": [1, 3, 5]",
    "77-0": "**when.delivery_timezone\\***",
    "77-1": "To adapt delivery times to user or account's timezone.",
    "77-2": "Optional",
    "77-3": "Boolean",
    "77-4": "\"delivery_timezone\":\"user\"  \nOR  \n\"delivery_timezone\":\"account\"",
    "78-0": "**when.user_timezone_wrap_around\\***",
    "78-1": "If the campaign start time is after a user's local time, then drop the campaign (user_timezone_wrap_around : false) or deliver the campaign the next day (user_timezone_wrap_around : true).",
    "78-2": "Optional",
    "78-3": "Boolean",
    "78-4": "\"user_timezone_wrap_around\":true",
    "79-0": "**dnd\\***",
    "79-1": "Do Not Disturb",
    "79-2": "Optional",
    "79-3": "Object",
    "79-4": "\"dnd\": {  \n  \"message_state\": \"delay\",  \n  \"dnd_timezone\": \"user\",  \n  \"dnd_info\": {  \n    \"2\": [\"21:00\", \"06:00\"],  \n    \"4\": [\"23:56\", \"04:34\"]  \n  }  \n}",
    "80-0": "**dnd.message_state\\***",
    "80-1": "The following are the values for parameter:  \n\"delay\": Delay message send until the end of DND.  \n\"discard\" : Discard messages scheduled during DND.  \nNote: By default, messages scheduled during DND are discarded (if message_state is not provided).",
    "80-2": "Required if DND applicable.",
    "80-3": "Integer",
    "80-4": "\"message_state\":\"discard\"",
    "81-0": "**dnd.dnd_info\\***",
    "81-1": "Provides the following information:  \nFormat: [Start Time in minutes, End Time in minutes]  \nDay: Denotes the days of week on which the DND is applicable.  \nFormat: [Sun to Sat] is [1 to 7] value",
    "81-2": "Required if DND applicable",
    "81-3": "Object",
    "81-4": "\"dnd_info\": {  \n  \"2\": [\"21:00\", \"06:00\"],  \n  \"4\": [\"23:56\", \"04:34\"]  \n}",
    "82-0": "**dnd.dnd_timezone\\***",
    "82-1": "Timezone for DND (user_timezone or account_timezone)",
    "82-2": "Required if DND applicable",
    "82-3": "String",
    "82-4": "\"dnd_timezone\":\"user\",  \nOR  \n\"dnd_timezone\":\"account\",",
    "83-0": "**when.campaign_cutoff\\***",
    "83-1": "Stop sending messages from this campaign after a particular time (Campaign Cutoff)  \nFormat : \"HH:MM\"",
    "83-2": "Optional",
    "83-3": "String (Time)",
    "83-4": "\"campaign_cutoff\": \"12:20\""
  },
  "cols": 5,
  "rows": 84,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


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
            "safari": {
                "deep_link": "https://www.google.com",
                "ttl":10
            },
            "chrome": {
              "image": "https://www.exampleImage.com",
              "icon":"https://www.exampleIcon.com",
              "deep_link": "http://www.example.com",
              "ttl":10,
              "require_interaction":true,
              "cta_title1":"title",
              "cta_link1":"http://www.example2.com",
              "cta_iconlink1":"https://www.exampleIcon2.com"
            },
            "firefox": {
            	"icon":"https://www.exampleIcon.com",
                "deep_link": "https://www.google.com",
                "ttl":10
            },
            "kaios": {
              "icon":"https://www.exampleIcon.com",
              "ttl":10,
              "kaiosKV": {
                "key1": "value1",
                "key2": "value2"
              }
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
    "conversion_goal":{
      "event_name": "Charged",
      "filter_type":{
          "event_property":[
              {
                   "property_name":"Items|Book name",
                   "operator":"equals",
                   "property_value":"book name"
               },
               {
                   "property_name":"Amount",
                   "operator":"equals",
                   "property_value":3456
               },
               {
                   "property_name":"Currency",
                   "operator":"equals",
                   "property_value":"USD"
               }
           ],
           "first_time":"yes",
           "last_time":"yes",
           "time_of_day":[["21:00","23:00"],["11:34","12:44"],["13:01","13:40"]],
           "day_of_week":[1,7],
           "day_of_month":[1,3,16,31]
      },
      "conversion_time":"1D",
      "revenue_property": "Amount"
   },
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
    "ttl": {
             "ttl_type": "absolute",
                "value" : 202207200000 },
    "content": {
        "title": "Hi!",
        "body": "How are you doing today?",
        "platform_specific": {
            "ios": {
                "mutable-content": "true", 
                "deep_link": "example.com",
                "sound_file": "example.caf",
                "category": "application category//Books",
                "badge_count": 1,
                "Key": "Value_ios"
            },
            "android": {
                "enable_rendermax": true,
                "wzrk_cid": "Marketing",
                "background_image": "http://example.jpg",
                "default_sound": true,
                "deep_link": "example.com",
                "large_icon": "http://example.png",
                "Key": "Value_android",
                },
            }
        }
    },
    "devices": [
        "android",
        "ios"
    ],
    "users_limit_overall": 101,
  "when":{
      "type":"now",
      "delivery_timezone":"user",
      "user_timezone_wrap_around":true,
      "dnd":{
          "message_state":"delay",
          "dnd_timezone":"user",
          "dnd_info":{
              "2": ["21:00","06:00"],
              "4": ["23:56","04:34"],
              "5": ["09:00","09:13"]
       }
      },
      "campaign_cutoff": "08:00"
  }
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
```json AMP Email
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
  "subject": "Weekly summary is ready!",
  "body": "<div>Hi Subscriber,<br>Your HTML email content goes here.</div>",
  "amp_body": "<!doctype html><html amp4email><head><meta charset='utf-8'></head><body><p>Hi Subscriber,</p><p>Your AMP email content goes here.</p></body></html>",
  "sender_name": "CleverTap"
    },
    "when": "now"
}
```
```json Email with Personalization
'{
  "name": "Personalized Email API Campaign",
  "target_mode": "email",
  "estimate_only": false,
  "where": {
    "event_name": "App Launched",
    "from": 20251001,
    "to": 20251112,
    "common_profile_properties": {
      "profile_fields": [
        {
          "name": "Plan Type",
          "operator": "equals",
          "value": "Premium"
        }
      ]
    }
  },
  "respect_frequency_caps": true,
  "content": {
    "subject": "Your {{ Profile.plan_type | default='Plan'}} plan just got better!",
    "body": "<div>Hey {{Profile.name | default='Subscriber'}},<br><br>We’ve rolled out new benefits for your plan, including early access to exclusive features and priority support.<br><br>Explore what’s new today and make the most of your membership.<br><br><a href='{{ Profile.account_url | default='https://www.ABC.com/login'}}' style='background:#4a90e2;color:#fff;padding:10px 16px;border-radius:6px;text-decoration:none;'>View My Account</a><br><br>Cheers,<br>The CleverTap Team</div>",
"amp_body": "<!doctype html><html amp4email><head><meta charset='utf-8'><style amp4email-boilerplate>body{visibility:hidden}</style></head><body><p>Hey {{ profile.name | default: 'Subscriber' }},</p><p>Your {{ profile.plan_type | default: 'Plan' }} plan just got better! We’ve introduced new benefits and early access features just for you.</p><p><a href='{{ profile.account_url | default: \"https://www.ABC.com/login\" }}'>View My Account</a></p><p>Cheers,<br>The CleverTap Team</p></body></html>",
    "sender_name": "CleverTap",
    "from_email": "updates@clevertap.com",
    "reply_to": "support@clevertap.com"
  },
  "when": "now"
}
```
```json SMS
{
    "name": "SMS Header,Message Key Values",
    "estimate_only": false,
    "target_mode": "sms",
    "where": {
        "event_name": "Notification Replied",
        "from": 20240810,
        "to": 20240910,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Phone",
                    "operator": "equals",
                    "value": "+130123456"
                }
            ]
        }
    },
    "provider_nick_name": "Provider Nickname",
    "respect_frequency_caps": false,
    "when": "now",
    "content": {
        "body": "Random Body",
        "message_info": {
            "1726822661": {
                "header_kvs": {
                    "$$header1": "Header 1",
                    "$$header2": "Header 2"
                },
                "message_kvs": {
                    "$$message1": "Message 1",
                    "$$message2": "Message 2"
                }
            }
        }
    }
}'
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
    "send_to_all_devices": true,
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
  	"custom_kv":[     //Supported for SDK version 5.2.0 or higher.
        {
           "key" :"k1",
           "value": {
               "text": ""
               },
           "android" : true,
           "ios":true
        },
         {
           "key" :"k2",
           "value": {
               "text": "someValueApi2"
               },
           "android" : true
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
    "send_to_all_devices": true,
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
  "custom_kv":[     //Supported for SDK version 5.2.0 or higher.
        {
           "key" :"k1",
           "value": {
               "text": ""
               },
           "android" : true,
           "ios":true
        },
         {
           "key" :"k2",
           "value": {
               "text": "someValueApi2"
               },
           "android" : true
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
    "send_to_all_devices": true,
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
    "custom_kv":[     //Supported for SDK version 5.2.0 or higher.
        {
           "key" :"k1",
           "value": {
               "text": ""
               },
           "android" : true,
           "ios":true
        },
         {
           "key" :"k2",
           "value": {
               "text": "someValueApi2"
               },
           "android" : true
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
    "send_to_all_devices": true,  
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
 		 "custom_kv":[     //Supported for SDK version 5.2.0 or higher.
        {
           "key" :"k1",
           "value": {
               "text": ""
               },
           "android" : true,
           "ios":true
        },
         {
           "key" :"k2",
           "value": {
               "text": "someValueApi2"
               },
           "android" : true
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
```json Whatsapp
{
    "name": "Standard Whatsapp campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "provider_nick_name": "provider_name",
    "where": {
        
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "email",
                    "operator": "equals",
                    "value": "Apple"
                }
            ]
        }
    },
   "respect_frequency_caps":false,
   "system_control_group_include":false,
   "content":{
      "template_name":"otp",
      "locale": "en", 
      "header":{
         "type":"text",
         "replacements":["test"]
      },
      "body":{
         "replacements":[
            "Dummy User"
         ]
      },
       "buttons":[
         {
            "replacements":[
             "suffix"  
            ]
         }
      ]
   },
   "when":"now"
}
```

Following is the JSON payload for the iOS Carousel Push template:

```json
{
    "name": "My API Campaign",
    "estimate_only": true,
    "target_mode":"push",
    "where": {
        "event_name": "App Launched",
        "from": 20250101,
        "to": 20250303,
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
            "ios": {
                "mutable-content": "true", 
                "deep_link": "example.com",
                "sound_file": "example.caf",
                "category": "CTNotification",
                "badge_count": 1,
                "ct_ContentSlider": {
        "orientation": "landscape", // landscape assumes 16:9 images, remove to display default square/portrait images
        "showsPaging": true, // optional to display UIPageControl
        "autoPlay": true, // optional to auto play the slideshow
        "autoDismiss": true, // optional to auto dismiss the notification on item actionUrl launch
        "items":[
            {
                "caption": "caption one",
                "subcaption": "subcaption one",
                "imageUrl": "https://s3.amazonaws.com/ct-demo-images/landscape-1.jpg",
                "actionUrl": "com.clevertap.ctcontent.example://item/one"
            }, 
            {
                "caption": "caption two", 
                "subcaption": "subcaption two", 
                "imageUrl": "https://s3.amazonaws.com/ct-demo-images/landscape-2.jpg",
                "actionUrl": "com.clevertap.ctcontent.example://item/two"
            }
       ]
   }
            }
        }
    },
    "devices": [
        "ios"
    ],
    "when": "now"
}
```

Following is the JSON payload for WhatsApp interactive message templates:

> 📘 Note:
> 
> Currently, Create Campaign APIs for WhatsApp do not support Carousel templates, Limited Time Offer (LTO) templates, Custom key-value pairs,, and templates with copy code button.

```json Image Template
{
    "name": "Interactive Whatsapp Campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "provider_nick_name": "Provider_1", //Mandatory for WhatsApp
    "where": {
        "event_name": "Added To Cart",
        "from": 20200610,
        "to": 20200619,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Fruit",
                    "operator":"equals",
                    "value": "Apple"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    
    "system_control_group_include": false,
    "content": {
        "template_name": "662da022_5583_406d_9c71_428d5e3164bb:ticket_update",
        "locale": "en", //Mandatory for WhatsApp
        "header": {
            "type": "image",  
            "media": {
                "url": "https://googl.com/a.jpg/"
            }
        },
        "body": {
            "replacements": []
        }     
      },
    "when": "now"
}

```
```json Text Template
{
    "name": "Interactive Whatsapp Campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "provider_nick_name": "Provider_1", //Mandatory for WhatsApp
    "where": {
        "event_name": "Added To Cart",
        "from": 20200610,
        "to": 20200619,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Fruit",
                    "operator":"equals",
                    "value": "Apple"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    
    "system_control_group_include": false,
    "content": {
        "template_name": "662da022_5583_406d_9c71_428d5e3164bb:ticket_update",
        "locale": "en", //Mandatory for WhatsApp
        "header": {
            "type": "text",
            "replacements": ["Dummy User", "22/10/2021"]
        },
        "body": {
            "replacements": ["dummy"]
        }
       
      },
    "when": "now"
}

```
```json Document Template
{
    "name": "Interactive Whatsapp Campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "provider_nick_name": "Provider_1", //Mandatory for WhatsApp
    "where": {
        "event_name": "Added To Cart",
        "from": 20200610,
        "to": 20200619,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Fruit",
                    "operator":"equals",
                    "value": "Apple"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    
    "system_control_group_include": false,
    "content": {
        "template_name": "662da022_5583_406d_9c71_428d5e3164bb:ticket_update",
        "locale": "en", //Mandatory for WhatsApp
        "header": {
            "type": "document",   
            "media": {
                "url": "https://googl.com/a.pdf"
            }
        },
        "body": {
            "replacements": ["Dummy User", "22/10/2021"]
        }     
      },
    "when": "now"
}

```
```json Video Template
{
    "name": "Interactive Whatsapp Campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "provider_nick_name": "Provider_1", //Mandatory for WhatsApp
    "where": {
        "event_name": "Added To Cart",
        "from": 20200610,
        "to": 20200619,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Fruit",
                    "operator":"equals",
                    "value": "Apple"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    
    "system_control_group_include": false,
    "content": {
        "template_name": "662da022_5583_406d_9c71_428d5e3164bb:ticket_update",
        "locale": "en", //Mandatory for WhatsApp
        "header": {
            "type": "video",
            "media": {
                "url": "https://googl.com/a.mp4"
            }
        },
        "body": {
            "replacements": ["Dummy User", "22/10/2021"]
        }     
      },
    "when": "now"
}


```
```json Location
{
    "name": "Interactive Whatsapp Campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "provider_nick_name": "Provider_1", //Mandatory for WhatsApp
    "where": {
        "event_name": "Added To Cart",
        "from": 20200610,
        "to": 20200619,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Fruit",
                    "operator":"equals",
                    "value": "Apple"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    
    "system_control_group_include": false,
    "content": {
        "template_name": "662da022_5583_406d_9c71_428d5e3164bb:ticket_update",
        "locale": "en", //Mandatory for WhatsApp
        "header": {
            "type": "location",
            "media": {
                "latitude": 32.123,
                "longitude": 123.32,
                "name": "facebook ",
                "address": "facebook "
            }
        },
        "body": {
            "replacements": ["Dummy User", "22/10/2021"]
        }     
      },
    "when": "now"
}

```
```json Media Template With Dynamic CTA buttons
{
   "name":"Interactive Whatsapp Campaign",
   "estimate_only":false,
   "target_mode":"whatsapp",
   "provider_nick_name": "Provider_1", //Mandatory for WhatsApp
   "where":{
      "event_name":"Added To Cart",
      "from":20200610,
      "to":20200619,
      "common_profile_properties":{
         "profile_fields":[
            {
               "name":"Fruit",
               "operator":"equals",
               "value":"Apple"
            }
         ]
      }
   },
   "respect_frequency_caps":false,
   "system_control_group_include":false,
   "content":{
      "template_name":"662da022_5583_406d_9c71_428d5e3164bb:ticket_update",
      "locale": "en", //Mandatory for WhatsApp
      "header":{
         "type":"location",
         "media":{
            "latitude":32.123,
            "longitude":123.32,
            "name":"facebook ",
            "address":"facebook "
         }
      },
      "body":{
         "replacements":[
            "Dummy User",
            "22/10/2021"
         ]
      },
      "buttons":[
         {
            "replacements":[
             "suffix"  
            ]
         }
      ]
   },
   "when":"now"
}

```

## Labels

You can associate one or more labels with a campaign when creating it. Labels make it easier to organize, search, and filter campaigns in the CleverTap dashboard.

### Parameters

| **Parameter** | **Description**                                                                                                             | **Required/Optional** | **Type**         | **Example Value**    |
| ------------- | --------------------------------------------------------------------------------------------------------------------------- | --------------------- | ---------------- | -------------------- |
| labels        | A list of labels to attach to the campaign. These labels will be visible in the CleverTap dashboard under campaign details. | Optional              | Array of strings | `["news", "sports"]` |

### Example Payload with Labels

```json
{
  "name": "Test Labels",
  "estimate_only": false,
  "target_mode": "push",
  "labels": [
    "news",
    "sports"
  ],
  "where": {
    "event_name": "App Launched",
    "from": 20250901,
    "to": 20250931,
    "common_profile_properties": {
      "profile_fields": [
        {
          "name": "Customer Type",
          "operator": "equals",
          "value": "Platinum"
        }
      ]
    }
  },
  "respect_frequency_caps": false,
  "content": {
    "title": "Hi!",
    "body": "How are you doing today?"
  },
  "when": "now"
}
```

# SMS

The Generic Sample Campaign API includes configurations for without Key Values, Key Value in Header Body, Key Value in Message Body, Target Level Key Value & Template ID, with payloads added below.

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required/Optional",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "$$header1",
    "0-1": "The header key must match the key-value pairs defined in the Provider Setup headers.",
    "0-2": "Required",
    "0-3": "String",
    "0-4": "",
    "1-0": "$$message1",
    "1-1": "The message (body) key must match the key-value pairs defined in the Provider Setup body",
    "1-2": "Required",
    "1-3": "String",
    "1-4": "",
    "2-0": "1726822661",
    "2-1": "This is the provider ID given by CleverTap, and you can obtain it from the URL when viewing the provider details in the dashboard or you can reach out to support team or account manager to get this value.",
    "2-2": "Required",
    "2-3": "String",
    "2-4": "",
    "3-0": "name",
    "3-1": "The name of your campaign shown in the CleverTap dashboard.",
    "3-2": "Required",
    "3-3": "String",
    "3-4": "“My Campaign”",
    "4-0": "provider_nickname",
    "4-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is only for Email, WhatsApp, and SMS.",
    "4-2": "Required",
    "4-3": "String",
    "4-4": "\"Provider 1\"",
    "5-0": "where",
    "5-1": "Allows you to filter target base on the user events and profile properties. Send an empty object ({}) to target your entire user base.",
    "5-2": "Required",
    "5-3": "Object",
    "5-4": "\"where\": {  \n\"event_name\": \"Charged\",  \n\"from\": 20251001,  \n\"to\": 20251220,  \n\"common_profile_properties\": {  \n\"profile_fields\": ed/Optional\",  \n    \"h-3\": \"Type\",  \n    \"h-4\": \"Example Value\",  \n    \"0-0\": \"name\",  \n    \"0-1\": \"The name of your campaign shown in the CleverTap dashboard.\",  \n    \"0-2\": \"Required\",  \n  \n}  \n}",
    "6-0": "where.event_name",
    "6-1": "Allows you to target users based on an event they perform.",
    "6-2": "Optional",
    "6-3": "String",
    "6-4": "\"charged\"",
    "7-0": "where.from",
    "7-1": "Start of date range for event needed. Value specified in format YYYYMMDD.",
    "7-2": "Optional",
    "7-3": "int",
    "7-4": "20251001",
    "8-0": "where.to",
    "8-1": "End of date range for event needed. Value specified in format YYYYMMDD.",
    "8-2": "Optional",
    "8-3": "int",
    "8-4": "20251220",
    "9-0": "where.profile_fields",
    "9-1": "Allows you to target users based on the values of their profile fields.",
    "9-2": "Optional",
    "9-3": "int",
    "9-4": "\"common_profile_properties\": {  \n\"profile_fields\": \"h-0\": \"Parameter\",  \n    \"h-1\": \"Description\",  \n    \"h-2\": \"Required/Optional\",  \n    \"h-3\": \"Type\",  \n  \n}",
    "10-0": "when",
    "10-1": "When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.",
    "10-2": "Required",
    "10-3": "String",
    "10-4": "\"now\"",
    "11-0": "content.body",
    "11-1": "Dynamically replaces placeholder values in the body of interactive templates.",
    "11-2": "Required",
    "11-3": "Object",
    "11-4": ""
  },
  "cols": 5,
  "rows": 12,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


```json Basic SMS Campaign Payload
{
    "name": "Sms Campaign API",
    "estimate_only": true,
    "target_mode": "sms",
    "where": {
        "event_name": "Notification Replied",
        "from": 20240810,
        "to": 20240910,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Phone",
                    "operator": "equals",
                    "value": "+180123456"
                }
            ]
        }
    },
    "provider_nick_name": "Nick Name",
    "respect_frequency_caps": false,
    "when": "now",
    "content": {
        "body": "Hello User"
    }
}'
```
```json SMS Campaign with Header Key-Value Pairs
{
    "name": "Sms Campaign API",
    "estimate_only": false,
    "target_mode": "sms",
    "where": {
        "event_name": "Notification Replied",
        "from": 20240810,
        "to": 20240910,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Phone",
                    "operator": "equals",
                    "value": "+180123456"
                }
            ]
        }
    },
    "provider_nick_name": "Nick Name",
    "respect_frequency_caps": false,
    "when": "now",
    "content": {
        "body": "Hello User",
        "message_info": {
            "1726821834": {
                "header_kvs": {
                    "$$header1": "Header 1",
                    "$$header2": "Header 2"
                }
            }
        }
    }
}'
```
```json SMS Campaign with Body Key-Value Pairs
{
    "name": "Sms Campaign API",
    "estimate_only": false,
    "target_mode": "sms",
    "where": {
        "event_name": "Notification Replied",
        "from": 20240810,
        "to": 20240910,
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Phone",
                    "operator": "equals",
                    "value": "+180123456"
                }
            ]
        }
    },
    "provider_nick_name": "Nick Name",
    "respect_frequency_caps": false,
    "when": "now",
    "content": {
        "body": "Hello User",
        "message_info": {
            "1726822153": {
                "message_kvs": {
                    "$$message1": "Message 1",
                    "$$message2": "Message 2"
                }
            }
        }
    }
}'
```
```json SMS Campaign with  Template ID & Key-Value Pairs
{
    "name": "Target Key Value SMS",
    "estimate_only": false,
    "target_mode": "sms",
    "when": "now",
    "where": {
        "common_profile_properties": {
            "profile_fields": [
                {
                    "name": "Phone",
                    "operator": "equals",
                    "value": "+180123456"
                }
            ]
        }
    },
    "respect_frequency_caps": false,
    "provider_nick_name": "Nick Name",
    "content": {
        "body": "Hello user",
        "message_info": {
            "1726823920": {
                "template_id": "TEMPLATE ID",
                "header_kvs": {
                    "$$header1": "Header 1",
                    "$$header2": "Header 2"
                },
                "message_kvs": {
                    "targetkey1": "kv value 1",
                    "targetkey2": "kv value 2",
                    "$$MessageID": "message ID"
                }
            }
        }
    }
}'
```

## Example Request - Target User Events & Properties

Here is an example cURL request to the _Create Campaign_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d ' {"name": "My Sms API campaign","estimate_only": true,"target_mode":"sms", "where":{"event_name":"Charged","from":20251001,"to":20251220,"common_profile_properties":{"profile_fields": [ {"name": "Customer Type","operator":"equals","value": "Platinum"}]}},"respect_frequency_caps": false,"content": { "body": "Sms body"},"when": "now"}' \
"https://in1.api.clevertap.com/1/targets/create.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/targets/create.json ")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
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

response = requests.post('https://in1.api.clevertap.com/1/targets/create.json', headers=headers, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/targets/create.json', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/targets/create.json ',
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

```json
{
  "status": "success",
  "estimates": {
    "web": 2
  }
}
```

## Example Request - Event Occurrence

```json
{
    "advanced_query": {
        "did_all": [
            {
                "event_name": "App Launched",
                "from": 20200101,
                "to": 20200218,
                "operator": "greater_than_equals",
                "value": 5,
            }
        ]
    }
}
```

# Create Campaign API - Target Users by their Identities

You can send notifications to your users based on their Facebook ID, Email ID, custom-defined identity, or CleverTap ID.

You can message 1000 users per API request.

## Base URL - Target Users by their Identities

The endpoint is different based on the channel you are sending the message. 

Following are some example base URLs from the account in the India region:

**SMS**  
<https://in1.api.clevertap.com/1/send/sms.json>

**Push**  
<https://in1.api.clevertap.com/1/send/push.json>

> ❗️ Note:
> 
> The push.json API currently does not support Pull Notifications.

**Web Push**  
<https://in1.api.clevertap.com/1/send/webpush.json>

**Email**  
<https://in1.api.clevertap.com/1/send/email.json>

> 📘 Region
> 
> These are some example base URLs from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/create-campaign-api#region).

**WhatsApp**  
<https://in1.api.clevertap.com/1/send/whatsapp.json>

## HTTP Method - Target Users by their Identities

POST

## Headers - Target Users by their Identities

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

| Header                 | Description                                                                | Type   | Example Value                                  |
| :--------------------- | :------------------------------------------------------------------------- | :----- | :--------------------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                                                 | string | "X-CleverTap-Account-Id: ACCOUNT_ID"           |
| X-CleverTap-Passcode   | Your CleverTap Account Passcode.                                           | string | "X-CleverTap-Passcode: PASSCODE"               |
| Content-Type           | Request content-type should always set to application/json; charset=utf-8. | string | "Content-Type: application/json;charset=utf-8" |

## Body Parameters - Target Users by their Identities

The body is uploaded as a JSON payload. 

to, name, title, body, subject, sender_name, and  target_mode are required parameters. 

The to parameter is an object that has the following child properties: FBID, Email, Identity, objectId, which are used as identifiers to target your users. Each request can contain a max of 1000 users across these identifiers. 

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
    "0-4": "\"to\": {  \n        \"FBID\":   \"h-0\": \"Parameter\",  \n    \"h-1\",  \n  \n`    \"Email\": ,\n\"h-4\": \"Example Value\",\n\"0-0\": \"to\",\n\"0-1\",\n    \"Identity\": \",\n\"0-3\": \"object\",\n\"0-4\",\n    \"objectId\": ,\n\"1-1\": \"Facebook ID for user.\",\n\"1-2\": \"required\",\n\"1-3\"\n}`",
    "1-0": "to.FBID",
    "1-1": "Facebook ID for user.",
    "1-2": "required",
    "1-3": "array of strings",
    "1-4": "\"to\": {  \n        \"FBID\":   \"h-0\": \"Parameter\",  \n    \"h-1\"  \n}",
    "2-0": "to.Email",
    "2-1": "Email identifier for user.",
    "2-2": "required",
    "2-3": "array of strings",
    "2-4": "{  \n    \"to\": {  \n        \"Email\": h-0\": \"Parameter\",  \n    \"h-1\": \"Description\",  \n    \"h-2\"  \n}",
    "3-0": "to.Identity",
    "3-1": "Custom-defined identifier for user.",
    "3-2": "required",
    "3-3": "array of strings",
    "3-4": "{  \n    \"to\": {  \n      \"Identity\": -0\": \"Parameter\",  \n    \"h-1\"  \n}",
    "4-0": "to.objectId",
    "4-1": "CleverTap identifier for user.",
    "4-2": "required",
    "4-3": "array of strings",
    "4-4": "{  \n    \"to\": {  \n        \"objectId\": \": \"Parameter\",  \n    \"h-1\"  \n}",
    "5-0": "tag_group",
    "5-1": "<li>Each message can be tagged into a tag group.</li><li>Metrics, such as sent and viewed, will be shown against each tag group in the CleverTap dashboard.</li><li>You can create a maximum of 500 tag groups.</li></li> If not specified, the metrics are shown against the “Not Tagged” tag.</li><li? The _Stats_ page shows the tag group for the selected campaign.</li><li>For campaigns created via the Target Users by Identity API with tag groups, CleverTap strongly recommends setting up individual subscriptions. This approach ensures that even if a campaign contains many tag groups, it does not affect row allocation for other campaigns.</li>",
    "5-2": "optional (either use tag \\_group or campaign_id)",
    "5-3": "string",
    "5-4": "\"my tag group\"",
    "6-0": "content",
    "6-1": "Object that defines the content for your message.",
    "6-2": "required",
    "6-3": "object",
    "6-4": "\"content\": {  \n        \"subject\": \"Welcome\",  \n        \"body\": \"<div>Your HTML content for the email</div>\",  \n        \"sender_name\": \"CleverTap\"  \n    }",
    "7-0": "content.title",
    "7-1": "Title content of your push notification message and web push message which is sent to the user.",
    "7-2": "required",
    "7-3": "string",
    "7-4": "\"Hi\"",
    "8-0": "content.body",
    "8-1": "Body content of your push notification, web push, email and sms which is sent to the user.",
    "8-2": "required",
    "8-3": "string",
    "8-4": "“Have you seen the special offer?”",
    "9-0": "content.subject",
    "9-1": "Subject content of your email which is sent to the user.",
    "9-2": "required",
    "9-3": "string",
    "9-4": "\"Subject\"",
    "10-0": "content.sender_name",
    "10-1": "Sender name for your email which is sent to the user.",
    "10-2": "required",
    "10-3": "string",
    "10-4": "“Onboarding”",
    "11-0": "content.message_type ",
    "11-1": "Type of the  message. These can be two types:                 1. Template - Use when you are using a template to trigger the message               2. Freeform - When you are replying back to the user. ",
    "11-2": "Required (Applicable only for WhatsApp)",
    "11-3": "String",
    "11-4": "\"template\" or \"Freeform\"",
    "12-0": "content.template",
    "12-1": "Contains all the information about the message content in the template. ",
    "12-2": "Required(Applicable only for WhatsApp)",
    "12-3": "Object",
    "12-4": " \"template\": {  \n            \"template_name\": \"Image_Template_1\",  \n              \"header\": {  \n                \"type\": \"image\",  \n                \"media\": {  \n                    \"url\": \"<https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg\">  \n                }  \n            },  \n            \"body\": {  \n                \"type\": \"text\",  \n                \"replacements\": [\"var1\",\"Var2\"]  \n            },  \n  \n`        \"buttons\": \\[\n            {\n                \"replacements\": [  \n                    \"mvi qr payload 5\"  \n                ]\n            },\n            {\n                \"replacements\": [  \n                    \"mvi qr payload 7\"  \n                ]\n            }\n        ]`}",
    "13-0": "content.template.template_name ",
    "13-1": "Name of the template used",
    "13-2": "Required(Applicable only for WhatsApp)",
    "13-3": "String",
    "13-4": "\"video_Template_1\"",
    "14-0": "content.template.language",
    "14-1": "Language code for the language used to create the template. For more information, see [Meta Message Templates](https://developers.facebook.com/docs/whatsapp/api/messages/message-templates/#supported-languages)",
    "14-2": "Required(Applicable only for WhatsApp)",
    "14-3": "String",
    "14-4": "\"en\"",
    "15-0": "content.template.header",
    "15-1": "Content information about the header used in the template. Used only if the template is using headers.",
    "15-2": "Required (Applicable only for WhatsApp) ",
    "15-3": "Object",
    "15-4": "\"header\": {  \n                \"type\": \"image\",  \n                \"media\": {  \n                    \"url\": \"<https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg\">  \n                }  \n            },  \nFor more header types, refer to the examples. ",
    "16-0": "content.template.header.type",
    "16-1": "Type of the header (for example, video, image, location, text, or document).",
    "16-2": "Required (Applicable only for WhatsApp)",
    "16-3": "String",
    "16-4": "\"video\"",
    "17-0": "content.template.header.media",
    "17-1": "Media-related information (URL of the video)",
    "17-2": "Required (Applicable only for WhatsApp)",
    "17-3": "Object",
    "17-4": "\"header\": {  \n                \"type\": \"image\",  \n                \"media\": {  \n                    \"url\": \"<https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg\">  \n                }  \n            },  \nFor more header types, refer to the examples.",
    "18-0": "content.template.header.replacements",
    "18-1": "Replacement Object for text headers. Applicable only for text headers. ",
    "18-2": "Required (Applicable only for WhatsApp)",
    "18-3": "Object",
    "18-4": "\"header\": {  \n                \"type\": \"text\",  \n                \"replacements\": [ \"header var 1\"   ]  \n  \n`        },`",
    "19-0": "content.template.body",
    "19-1": "Body section of the template",
    "19-2": "Required (Applicable only for WhatsApp)",
    "19-3": "Object",
    "19-4": "\"body\": {  \n                \"type\": \"text\",  \n                \"replacements\": [  \n                    \"Var 1\", \"Var 2\"  \n                ]  \n            }",
    "20-0": "content.template.body.type ",
    "20-1": "Type of the body (text in this case)",
    "20-2": "Required(Applicable only for WhatsApp)",
    "20-3": "String",
    "20-4": "\"text\"",
    "21-0": "content.template.body.replacements",
    "21-1": "Replacement variables in the body. A replacement value is required for each dynamic variable in the body. ",
    "21-2": "Required(Applicable only for WhatsApp)",
    "21-3": "Array",
    "21-4": "[\"var1\", \"Var2\"]",
    "22-0": "content.template.buttons",
    "22-1": "Button-related information",
    "22-2": "Required(Applicable only for WhatsApp)",
    "22-3": "Array",
    "22-4": "\"buttons\": \\[  \n                {  \n                    \"replacements\": [  \n                        \"quick reply custom payload 1\"  \n                    ]  \n                },  \n                {  \n                    \"replacements\": [  \n                        \"quick reply custom payload 2\"  \n                    ]  \n                }  \n            ]",
    "23-0": "provider_nick_name",
    "23-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is required for WhatsApp and SMS.",
    "23-2": "Required",
    "23-3": "string",
    "23-4": "\"Provider 1\"",
    "24-0": "respect_frequency_caps",
    "24-1": "Set to false if you want to override frequency caps.",
    "24-2": "optional",
    "24-3": "boolean",
    "24-4": "false",
    "25-0": "send_email_to_opted_out_users",
    "25-1": "Valid values are: true and false. Set to false by default. When true, emails are sent to all qualified users, including those who have unsubscribed. For more information, refer to [Override Communication Preferences for Email](https://docs.clevertap.com/docs/create-message-email#override-communication-preferences-for-email).",
    "25-2": "optional",
    "25-3": "boolean",
    "25-4": "\"send_email_to_opted_out_users\":true",
    "26-0": "wzrk_cid",
    "26-1": "Platform specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.  \n  \nIf a Notification ID is mandatory then the channel ID is expected. An error occurs if a valid channel ID is not received.",
    "26-2": "mandatory",
    "26-3": "string",
    "26-4": "",
    "27-0": "badge_id",
    "27-1": "Platform specific key that can be used while targeting users on devices running Android O - can be used to determine the notification count shown on the app icon",
    "27-2": "optional",
    "27-3": "int",
    "27-4": "",
    "28-0": "badge_icon",
    "28-1": "Platform specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.  \n  \nAccepted values  \n`app icon`  \n`notification icon`",
    "28-2": "optional",
    "28-3": "string",
    "28-4": "",
    "29-0": "mutable-content",
    "29-1": "To raise Notification viewed event for Push notifications, raise the flag to \"true\" in the platform_specific: ios section of the request payload.",
    "29-2": "optional",
    "29-3": "boolean",
    "29-4": "",
    "30-0": "platform_specific",
    "30-1": "The platform-specific details for Push and Web Push notification",
    "30-2": "optional",
    "30-3": "string",
    "30-4": "",
    "31-0": "background_image",
    "31-1": "The parameter to render images in the push notification for Android.",
    "31-2": "Optional",
    "31-3": "string",
    "31-4": "",
    "32-0": "ct_mediaUrl",
    "32-1": "The parameter to render images in the push notification for iOS.",
    "32-2": "Optional",
    "32-3": "string",
    "32-4": "",
    "33-0": "wzrk_clr",
    "33-1": "A Platform specific key that can be used to set color for small icons in push notifications.",
    "33-2": "optional",
    "33-3": "string",
    "33-4": "\\# 1DA1F2",
    "34-0": "respect_communication_preference",
    "34-1": "Send this value as _False_ to override the communication preferences of the user. For example, transaction updates, user replies, and so on. Check that you do not breach any compliance regulations by overriding the user communication preferences. ",
    "34-2": "Optional (Applicable only for WhatsApp)",
    "34-3": "Boolean",
    "34-4": "False",
    "35-0": "message_id",
    "35-1": "A unique identifier to track the status of each message sent to the end user. The limit is 8 characters.",
    "35-2": "Required(Applicable only for WhatsApp)",
    "35-3": "String",
    "35-4": "\"4536tsytr\""
  },
  "cols": 5,
  "rows": 36,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


Below are example payloads for web push, push, email, SMS, and WhatsApp.

```json Web Push
{
    "to": {
        "FBID": [
            "102029292929388",
            "114342342453463"
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
            "safari": {
                "deep_link": "https://www.google.com",
                "ttl":10
            },
            "chrome": {
              "image": "https://www.exampleImage.com",
              "icon":"https://www.exampleIcon.com",
              "deep_link": "http://www.example.com",
              "ttl":10,
              "require_interaction":true,
              "cta_title1":"title",
              "cta_link1":"http://www.example2.com",
              "cta_iconlink1":"https://www.exampleIcon2.com"
            },
            "firefox": {
            	"icon":"https://www.exampleIcon.com",
                "deep_link": "https://www.google.com",
                "ttl":10
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
            "ios": {
                "deep_link": "example.com",
                "sound_file": "example.caf",
                "mutable-content": "true", 
                "ct_mediaUrl": "https://i.ibb.co/pJ8D5Z7/woman-g40aa99db5-1920.jpg",
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
    "send_email_to_opted_out_users":true,
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
```json WhatsApp
{
  "to": {
    "Identity": [
      "johndoe@mail.com"
    ]
  },
  "tag_group": "Tag Group 1",
  "provider_nick_name": "Provider_1",
  "respect_frequency_caps": false,
  "message_id": "4536tsytr",
  "respect_communication_preference": true,
  "content": {
    "message_type": "template",
    "template": {
      "template_name": "video_Template_1",
      "header": {
        "type": "video",
        "media": {
          "url": "https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.mp4"
        }
      },
      "body": {
        "type": "text",
        "replacements": [
          "var1",
          "Var2"
        ]
      },
      //dynamic redirection CTA
      "buttons": [
        {
          "replacements": [
            "CTA_var"
          ]
        }
      ],
      "language": "en"
    }
  }
}
```

Following is the JSON payload for WhatsApp interactive message templates:

> 📘 Note:
> 
> Currently, Create Campaign APIs for WhatsApp do not support Carousel templates, Limited Time Offer (LTO) templates, Custom key-value pairs, and templates with copy code button.

```json Image Template
{
  "to": {
    "Identity": [
      "johndoe@mail.com"
    ]
  },
  "tag_group": "Tag Group 1",
  "provider_nick_name": "Provider_1",
  "respect_frequency_caps": false,
  "message_id": "4536tsytr",
  "respect_communication_preference": true,
  "content": {
    "message_type": "template",
    "template": {
      "template_name": "Image_Template_1",
      "header": {
        "type": "image",
        "media": {
          "url": "https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg"
        }
      },
      "body": {
        "type": "text",
        "replacements": [
          "var1",
          "Var2"
        ]
      },
      "language": "en"
    }
  }
}

```
```json Video Template
{
    "to": {
        "Identity": [
            "johndoe@mail.com"
        ]
    },
     "tag_group": "Tag Group 1",
    "provider_nick_name": "Provider_1",
    "respect_frequency_caps":false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_type": "template",
        "template": {
            "template_name": "video_Template_1",
            "language": "en",
              "header": {
                "type": "video",
                "media": {
                    "url": "https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.mp4"
                }
            },
            "body": {
                "type": "text",
                "replacements": ["var1","Var2"]
            }
          
           
        }
    }
}


```
```json Document Template
{
    "to": {
        "Identity": [
            "johndoe@mail.com"
        ]
    },
     "tag_group": "Tag Group 1",
    "provider_nick_name": "Provider_1",
    "respect_frequency_caps":false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_type": "template",
        "template": {
            "template_name": "document_Template_1",
            "language": "en",
              "header": {
                "type": "document",
                "media": {
                    "url": "http://www.ecommercecompany/allproducts/pdf/productcatalog.pdf"
                }
            },
            "body": {
                "type": "text",
                "replacements": ["var1","Var2"]
            }
          
           
        }
    }
}


```
```json Text Header with Quick Reply Button and Custom Button Payload
{
  "to": {
    "Identity": [
      "john@clevertap.com"
    ]
  },
  "tag_group": "Tag1",
  "provider_nick_name": "WhatsApp Provider",
  "message_id": "4764387",
  "respect_frequency_caps": false,
  "content": {
    "message_type": "template",
    "template": {
      "template_name": "shipping_order",
      "header": {
        "type": "text",
        "replacements": [
          "header var 1",
          "header var 2"
        ]
      },
      "body": {
        "type": "text",
        "replacements": [
          "1"
        ]
      },
                   // No need to mention footers info in API payload
            // Requirements are required only if you want to send custom payloads in the quick reply button. ds
      "buttons": [
        {
          "replacements": [
            "jack"
          ]
        }
      ],
      "language": "en_US"
    }
  }
}
```
```json Location
{
    "to": {
        "Identity": [
            "johndoe@mail.com"
        ]
    },
     "tag_group": "Tag Group 1",
    "provider_nick_name": "Provider_1",
    "respect_frequency_caps":false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_type": "template",
        "template": {
            "template_name": "location_Template_1",
            "language": "en",
              "header": {
                "type": "location",
                "media": {
                    "longitude": 80.82947469999999,
                    "latitude": 16.0174248,
                    "name": "johnhouse",
                    "address": "johnhouse, New York, 522265, India",
                    "url": "https://maps.google.com/?q=Repalle,+New+York+522265,+America&ftid=0x3a49f59a2391c549:0xfe237504b0eca607"
                }
            },
            "body": {
                "type": "text",
                "replacements": ["var1","Var2"]
            }
          
           
        }
    }
}

```
```json Media Template With Dynamic CTA buttons
{
  "to": {
    "Identity": [
      "johndoe@mail.com"
    ]
  },
  "tag_group": "Tag Group 1",
  "provider_nick_name": "Provider_1",
  "respect_frequency_caps": false,
  "message_id": "4536tsytr",
  "respect_communication_preference": true,
  "content": {
    "message_type": "template",
    "language": "en",
    "template": {
      "template_name": "Image_Template_1",
      "header": {
        "type": "image",
        "media": {
          "url": "https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg"
        }
      },
      "body": {
        "type": "text",
        "replacements": [
          "var1",
          "Var2"
        ]
      },
          //example CTA URL: www.example.com/{{1}}
      "buttons": [
        {
          "replacements": [
            "CTA"
          ]
        }
      ]
    }
  }
}
```
```json Media Templates With Custom Payloads
{
  "to": {
    "Identity": [
      "johndoe@mail.com"
    ]
  },
  "tag_group": "Tag Group 1",
  "provider_nick_name": "Provider_1",
  "respect_frequency_caps": false,
  "message_id": "4536tsytr",
  "respect_communication_preference": true,
  "content": {
    "message_type": "template",
    "language": "en",
    "template": {
      "template_name": "Image_Template_1",
      "header": {
        "type": "image",
        "media": {
          "url": "https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg"
        }
      },
      "body": {
        "type": "text",
        "replacements": [
          "var1",
          "Var2"
        ]
      },
      "buttons": [
        {
          "replacements": [
            "mvi qr payload 5"
          ]
        },
        {
          "replacements": [
            "mvi qr payload 7"
          ]
        }
      ]
    }
  }
}
```
```json Freeform Simple Text Message
{
  "to": {
    "Identity": [
      "johndoe@mail.com"
    ]
  },
  "tag_group": "whatsapp initial tag group",
  "provider_nick_name": "provider_1",
  "respect_frequency_caps": false,
  "message_id": "4536tsytr",
  "respect_communication_preference": true,
  "content": {
    "message_type": "freeform",
    "freeform": {
      "body": {
        "type": "text",
        "text": "Hi, How can we help you"
      }
    }
  }
}
```
```json Freeform Image Message
{
  "to": {
    "Identity": [
      "johndoe@mail.com"
    ]
  },
  "tag_group": "whatsapp initial tag group",
  "provider_nick_name": "provider_1",
  "respect_frequency_caps": false,
  "message_id": "4536tsytr",
  "respect_communication_preference": true,
  "content": {
    "message_type": "freeform",
    "freeform": {
      "body": {
        "type": "image",
        "media": {
          "url": "https://d2h5ndkimzo5f0.cloudfront.net/dist/1655455488/i/d024319ee2bf46cdbafbbe0d02afdb00.jpeg?v=1686580980",
          "caption": "example caption"
        }
      }
    }
  }
}
```
```json Freeform Video Message
{
    "to": {
        "Identity": [
           "johndoe@mail.com"
        ]
    },
    "tag_group": "whatsapp initial tag group",
    "provider_nick_name": "provider_1",
    "respect_frequency_caps": false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_type": "freeform",
        "freeform": {
            "body": {
                "type": "video",
                "media": {
                    "url": "https://d2h5ndkimzo5f0.cloudfront.net/dist/1655455488/i/d024319ee2bf46cdbafbbe0d02afdb00.mp4?v=1686580980",
                    "caption": "example caption"
                }
            }
        }
    }
}

```
```json Freeform Audio Message
{
    "to": {
        "Identity": [
           "johndoe@mail.com"
        ]
    },
    "tag_group": "whatsapp initial tag group",
    "provider_nick_name": "provider_1",
    "respect_frequency_caps": false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_type": "freeform",
        "freeform": {
            "body": {
                "type": "audio",
                "media": {
                    "url": "https://d2h5ndkimzo5f0.cloudfront.net/dist/1655455488/i/d024319ee2bf46cdbafbbe0d02afdb00.mp3?v=1686580980",
                }
            }
        }
    }
}

```
```json Freeform Document Message
{
    "to": {
        "Identity": [
           "johndoe@mail.com"
        ]
    },
    "tag_group": "whatsapp initial tag group",
    "provider_nick_name": "provider_1",
    "respect_frequency_caps": false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_type": "freeform",
        "freeform": {
            "body": {
                "type": "document",
                "media": {
                    "url": "https://d2h5ndkimzo5f0.cloudfront.net/dist/1655455488/i/31f19b65c9df40a2acc72034e5231922.pdf?v=1686589587",
                    "filename": "example.pdf"
                }
            }
        }
    }
}

```
```json Location Template
{
    "to": {
        "Identity": [
           "johndoe@mail.com"
        ]
    },
    "tag_group": "whatsapp initial tag group",
    "provider_nick_name": "provider_1",
    "respect_frequency_caps": false,
    "message_id": "4536tsytr",
    "respect_communication_preference":true,
    "content": {
        "message_id": "1234589",
        "message_type": "freeform",
        "freeform": {
            "body": {
                "type": "location",
                "media": {
                    "longitude": 80.82947469999999,
                    "latitude": 16.0174248,
                    "name": "johnhouse",
                    "address": "Johnhouse, New York, 522265, India",
                    "url": "https://maps.google.com/?q=Repalle,+New+York+522265,+America&ftid=0x3a49f59a2391c549:0xfe237504b0eca607"
                }
            }
        }
    }
}
```

## Example Request - Target Users by their Identities

Here is an example cURL request to the _Create Campaign_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"to":{"FBID":["102029292929388","114342342453463"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"campaign_id":1000000043,"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}' \
"https://in1.api.clevertap.com/1/send/sms.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/send/sms.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "to" => {
    "FBID" => [
      "102029292929388",
      "114342342453463"
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

data = '{"to":{"FBID":["102029292929388","114342342453463"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}'

response = requests.post('https://in1.api.clevertap.com/1/send/sms.json', headers=headers, data=data)
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
$data = '{"to":{"FBID":["102029292929388","114342342453463"],"Email":["john@doe.com","jane@doe.com"],"Identity":["JohnDoe"],"objectId":["_asdnkansdjknaskdjnasjkndja","-adffajjdfoaiaefiohnefwprjf"]},"tag_group":"mytaggroup","respect_frequency_caps":false,"content":{"body":"Smsbody"}}';
$response = Requests::post('https://in1.api.clevertap.com/1/send/sms.json', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/send/sms.json',
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

```json
{
    "status": "success",
    "message": "Added to queue for processing"
}
```

A successful response implies payload validity and addition to the processing queue. Message delivery is handled by the provider (push or email) and depends on if you provided a valid recipient id.

## Errors

If there are errors, you will receive a response in the format below. 

```json
{
    "status": "fail",
    "error": "<error message here>",
    "code": <error code here>
}
```

Following are the possible error codes:

[block:parameters]
{
  "data": {
    "h-0": "Error Code",
    "h-1": "Error Message",
    "h-2": "Troubleshooting Steps",
    "0-0": "21",
    "0-1": "\"to\" is a mandatory field\"",
    "0-2": "Check if the _to_ field in the payload is missing recipient records.",
    "1-0": "73",
    "1-1": "respect_frequency_caps",
    "1-2": "The value must be  Boolean.",
    "2-0": "76",
    "2-1": "\"tag_group\" has to be string",
    "2-2": "Check if the value for tag group is _string_. ",
    "3-0": "76",
    "3-1": "\"message_type\" has to be string",
    "3-2": "Check if the value for the Message Type is string.",
    "4-0": "78",
    "4-1": "Invalid payload pls update and try again",
    "4-2": "Invalid JSON payload. Verify the payload.",
    "5-0": "84",
    "5-1": "\"Invalid recipients\"",
    "5-2": "Recipient information mentioned in the payload does not exist in the CleverTap dashboard. ",
    "6-0": "89",
    "6-1": "Unexpected error, please try again",
    "6-2": "The message failed to send due to an unknown error.  \n  \nTry again. If the error persists, open a  support ticket from the dashboard.",
    "7-0": "89",
    "7-1": "The WhatsApp provider you've selected is not supported with your current WhatsApp add-on. To use this provider, please enable the WhatsApp Connect add-on",
    "7-2": "1\\. Check that you are not using external providers in the Send Message API .  \n2. Check your organization settings to determine if the  WhatsApp Connect Add-on is enabled for your project. If the error persists even if the add-on is enabled, please contact our support. ",
    "8-0": "89",
    "8-1": "The WhatsApp provider you've selected is not supported with your current WhatsApp add-on. To use this provider, please enable the WhatsApp Direct add-on",
    "8-2": "1\\. Check that you are not using CleverTap BSP in the Send Message API .  \n2. Check your organization settings to determine if the  WhatsApp Direct Add-on is enabled for your project. If the error persists even if the add-on is enabled, please contact our support.",
    "9-0": "89",
    "9-1": "\"provider_nick_name\" can't be empty",
    "9-2": "The provider nick name is mandatory for WhatsApp campaigns. Check if the provider_nick_name is missing or empty.",
    "10-0": "89",
    "10-1": "The provider_nick_name used in request does not exist in your account.",
    "10-2": "Check if the provider used in the message payload exists in the CleverTap dashboard. To check, navigate to Settings > Channel > WhatsApp. ",
    "11-0": "89",
    "11-1": "\"message_id\" length can't be greater than 8",
    "11-2": "Check if the length of the message ID is greater than 8 characters.",
    "12-0": "89",
    "12-1": "\"message_id\" can't be empty",
    "12-2": "Check if the value for the Message ID is missing.",
    "13-0": "89",
    "13-1": "Received invalid message type",
    "13-2": "The only accepted values are \"template\" or \"freeform\".",
    "14-0": "89",
    "14-1": "Template not found for the specified provider",
    "14-2": "Check if the template used in the payload exists for the specified provider. Navigate to Settings > Channel > WhatsApp> [provider name] to check the template. To fix this, either use a provider that has the specified template or use the template that is available for the provider mentioned in the payload. ",
    "15-0": "89",
    "15-1": "Template name is mandatory",
    "15-2": "Check if the template name is missing or empty.",
    "16-0": "89",
    "16-1": "Invalid number of body/header/button replacements specified, the body/header/button has [n] replacement(s).",
    "16-2": "The number of variables and values in the payload must match for body, header, or button. ",
    "17-0": "400",
    "17-1": "Invalid syntax. Rectify and try again. Read more: <https://developer.clevertap.com/docs/create-campaign-api>",
    "17-2": "The request failed because the request was malformed."
  },
  "cols": 3,
  "rows": 18,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


## View Campaigns Created Through APIs

To view campaigns created through APIs:

1. Navigate to the _Campaigns_ section from the CleverTap dashboard. The list of campaign displays.
2. Click the Filter icon > _Created by_ filter and select the _serverapi<channel name> _ option.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ee5427a-small-Filter_campaigns_by_API.png",
        null,
        "Filter API Campaigns"
      ],
      "align": "center",
      "border": true,
      "caption": "Filter API Campaigns"
    }
  ]
}
[/block]


3. Click **Apply**. All the related campaigns are displayed. 
4. Click any of the campaigns to view details and stats.

# Unsubscribe Link for Emails

If you’re using Amazon SES or your own SMTP gateway, you can follow the steps [listed on this page](https://docs.clevertap.com/docs/handling-unsubscribes) to enable your users to unsubscribe from your email notifications.

# FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
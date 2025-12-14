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
    "1-0": "provider_nick_name",
    "1-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is required for WhatsApp.",
    "1-2": "Optional (required for WhatsApp)",
    "1-3": "String",
    "1-4": "\"Provider 1\"",
    "2-0": "provider_group_nickname",
    "2-1": "Allows you to specify which provider group to use for your SMS campaign. You must use the name as added in your account settings",
    "2-2": "Optional",
    "2-3": "String",
    "2-4": "\"NewTestSMSProviderGroup\"",
    "3-0": "content.sms.template_id",
    "3-1": "Template ID (required for sending SMS to phone numbers of India)",
    "3-2": "Optional",
    "3-3": "String",
    "3-4": "",
    "4-0": "**conversion_goal\\***",
    "4-1": "Checks the end goal of the campaign for conversion tracking.",
    "4-2": "Optional",
    "4-3": "Object",
    "4-4": "\"conversion_goal\":{  \n\"event_name\": \"Charged\",  \n\"filter_type\":{  \n\"event_property\":\\[  \n{  \n\"property_name\":\"Items|Book name\",  \n\"operator\":\"equals\",  \n\"property_value\":\"book name\"  \n},  \n{  \n\"property_name\":\"Items|Category\",  \n\"operator\":\"equals\",  \n\"property_value\":\"category\"  \n},  \n{  \n\"property_name\":\"Payment mode\",  \n\"operator\":\"equals\",  \n\"property_value\":\"UPI\"  \n},  \n{  \n\"property_name\":\"Amount\",  \n\"operator\":\"between\",  \n\"property_value\":TestSMSProv  \n},  \n],  \n\"first_time\":\"yes\",  \n\"last_time\":\"yes\",  \n\"time_of_day\":\\[   \"4-0\": \"\\*\\*conv],  \n\"day_of_week\":conve,  \n\"day_of_month\":cks the end  \n},  \n\"conversion_time\":\"3D\",  \n\"revenue_property\": \"Amount\"  \n},",
    "5-0": "**conversion_goal.event_name\\***",
    "5-1": "Name of the goal you want to track.",
    "5-2": "Required if conversion_goal is provided",
    "5-3": "String",
    "5-4": "\"event_name\": \"Charged\",",
    "6-0": "**conversion_goal.filter_type\\***",
    "6-1": "Filters for provided event_name  \nThe type of filter applied in conversion tracking. For example, first_time, time_of_day and so on.",
    "6-2": "Required if conversion_goal is provided",
    "6-3": "Object",
    "6-4": "\"filter_type\":{  \n\"event_property\": \"h-0\": \"Parameter\",  \n    \"h-1\": \"Description\",  \n    \"h-2\": \"Required/Optional\",  \n    \"h-3\": \"Type\",  \n ,  \n\"first_time\":\"yes\",  \n\"last_time\":\"yes\",  \n\"time_of_day\":quired\",  \n    \"0-3,  \n\"day_of_week\":ring\",  \n\"day_of_month\": Campaign”\"  \n},",
    "7-0": "**conversion_goal. filter_type.  \nevent_property\\***",
    "7-1": "Event Property for given event_name.",
    "7-2": "Optional",
    "7-3": "List",
    "7-4": "\"event_property\":]  \n{  \n  \"data\": {  \n    \"h-0\": \"Parameter\",  \n    \"h-1\": \"Description\",  \n    \"h-2\": \"Required/Optional\",  \n ,",
    "8-0": "**conversion_goal.filter_type.  \nevent_property.property_name\\***",
    "8-1": "Name of the event_property.",
    "8-2": "Optional",
    "8-3": "String",
    "8-4": "\"property_name\":\"Items|Book name\",",
    "9-0": "**conversion_goal.filter_type.  \nevent_property.operator\\***",
    "9-1": "Operator required for property_name and property_value.  \nValid values are:  \nequals  \ngreater_than  \ngreater_than_equals  \nless_than  \nless_than_equals  \ncontains  \ndoes_not_contain  \nnot_equals  \nexists (does not require property_value)  \ndoes_not_exist (does not require property_value)  \nbetween (requires 2 values in an array)",
    "9-2": "Optional",
    "9-3": "String",
    "9-4": "\"operator\":\"equals\",",
    "10-0": "**conversion_goal.filter_type.  \nevent_property.property_value\\***",
    "10-1": "Provides value of the event_property.",
    "10-2": "Optional",
    "10-3": "String",
    "10-4": "\"property_value\":\"book name\"",
    "11-0": "**conversion_goal.  \nfilter_type.first_time\\***",
    "11-1": "Provides value of the first_time.  \nValid values are:  \n\"yes\"  \n\"no\"",
    "11-2": "Optional",
    "11-3": "String",
    "11-4": "\"first_time\":\"yes\",",
    "12-0": "**conversion_goal.  \nfilter_type.last_time\\***",
    "12-1": "Provides value of last_time.  \nValid values:  \n\"yes\"  \n\"no\"",
    "12-2": "Optional",
    "12-3": "String",
    "12-4": "\"last_time\":\"yes\",",
    "13-0": "**conversion_goal.  \nfilter_type.time_of_day\\***",
    "13-1": "Provides value of time_of_day.  \nValid values:  \n\\[ data\": {  \n    \"h-0\":  , rameter\",  \n    \"h-1\": ]  \ntime: \"HH:MM\"",
    "13-2": "Optional",
    "13-3": "List of lists",
    "13-4": "\"time_of_day\":\\[s]  \n{  \n  \"data\": {  \n,   \"h-0\": \"Parame,er\",  \n    \"h-1\": \"],",
    "14-0": "**conversion_goal.  \nfilter_type.day_of_week\\***",
    "14-1": "Provides value of day_of_week.  \nValid values:  \n  \"data\" is     \"h-0\": \"",
    "14-2": "Optional",
    "14-3": "List",
    "14-4": "\"day_of_week\":ers]  \n{",
    "15-0": "**conversion_goal.  \nfilter_type.day_of_month\\***",
    "15-1": "Provides value of day_of_month.  \nValid values:  \n  \"data\": {  \n    \"h-0\"  \nrange: 0\": \"Para",
    "15-2": "Optional",
    "15-3": "List",
    "15-4": "\"day_of_month\":rs]  \n{  \n  \"da",
    "16-0": "**conversion_goal.conversion_time\\***",
    "16-1": "Denotes the period that conversions are tracked for once the campaign is published.  \nAccepted values:  \n  \"data\": {  \n    \"h-0\": \"Parameter\",  \n    \"h-1\": \"Description\",  \n  \nwhere, m: minutes, H: hour, D: day, W: week, M:month",
    "16-2": "Required if conversion_goal is provided",
    "16-3": "String",
    "16-4": "\"conversion_time\":\"3D\",",
    "17-0": "**conversion_goal.revenue_property\\***",
    "17-1": "Denotes the revenue property for conversion goal event. For example, amount, currency, payment mode and so on.",
    "17-2": "Required if conversion_goal is provided",
    "17-3": "String",
    "17-4": "\"revenue_property\": \"Amount\"",
    "18-0": "where",
    "18-1": "Allows you to filter target base on the user events and profile properties. Send an empty object ({}) to target your entire user base.",
    "18-2": "Required",
    "18-3": "Object",
    "18-4": "\"where\": {  \n\"event_name\": \"Charged\",  \n\"from\": 20171001,  \n\"to\": 20171220,  \n\"common_profile_properties\": {  \n\"profile_fields\": ed/Optional\",  \n    \"h-3\": \"Type\",  \n    \"h-4\": \"Example Value\",  \n    \"0-0\": \"name\",  \n    \"0-1\": \"The name of your campaign shown in the CleverTap dashboard.\",  \n    \"0-2\": \"Required\",  \n  \n}  \n}",
    "19-0": "where.event_name",
    "19-1": "Allows you to target users based on an event they perform.",
    "19-2": "Optional",
    "19-3": "String",
    "19-4": "\"Charged\"",
    "20-0": "where.from",
    "20-1": "Start of date range for event needed. Value specified in format YYYYMMDD.",
    "20-2": "Optional",
    "20-3": "Int",
    "20-4": "20171001",
    "21-0": "where.to",
    "21-1": "End of date range for event needed. Value specified in format YYYYMMDD.",
    "21-2": "Optional",
    "21-3": "Int",
    "21-4": "20171220",
    "22-0": "where.profile_fields",
    "22-1": "Allows you to target users based on the values of their profile fields.",
    "22-2": "Optional",
    "22-3": "Int",
    "22-4": "\"common_profile_properties\": {  \n\"profile_fields\": \"h-0\": \"Parameter\",  \n    \"h-1\": \"Description\",  \n    \"h-2\": \"Required/Optional\",  \n    \"h-3\": \"Type\",  \n  \n}",
    "23-0": "estimate_only",
    "23-1": "If this parameter is set to true, the request returns an estimated reach of the campaign, which is the number of users who receive the notification when you send it out. Setting this parameter to true does not create the campaign.",
    "23-2": "Optional",
    "23-3": "Boolean",
    "23-4": "FALSE",
    "24-0": "async_estimate",
    "24-1": "This allows setting of the estimate_only flag asynchronously. If this parameter is set to true, it returns a request ID that can be passed in the subsequent API call.",
    "24-2": "Optional",
    "24-3": "Boolean",
    "24-4": "Boolean",
    "25-0": "system_control_group_include",
    "25-1": "If the system control group is enabled from the dashboard, you can choose to use the control group for API campaigns.",
    "25-2": "Optional",
    "25-3": "Boolean",
    "25-4": "",
    "26-0": "control_group",
    "26-1": "If the custom control group is enabled from the dashboard, you can choose to use the control group for API campaigns",
    "26-2": "Optional",
    "26-3": "String",
    "26-4": "\"control_group\": {  \n\"type\": \"custom\"  \n\"name\": \"name of control group\"  \n}",
    "27-0": "control_group",
    "27-1": "You can create a campaign control group via API by specifying the % as described on the left",
    "27-2": "Optional",
    "27-3": "Integer",
    "27-4": "\"control_group\": {  \n\"type\": \"campaign\"  \n\"percentage\": 5  \n}",
    "28-0": "skip_estimate",
    "28-1": "Toggle this flag to true if you do not want to estimate the reach. This will lead to faster campaign delivery.",
    "28-2": "Optional",
    "28-3": "Boolean",
    "28-4": "\"skip_estimate\":true",
    "29-0": "send_to_all_devices",
    "29-1": "The flag is set to false by default. If no value is specified, then the notification is sent to the last active device. Toggle this flag to true if you want to send notification to all active devices.",
    "29-2": "Optional",
    "29-3": "Boolean",
    "29-4": "FALSE",
    "30-0": "segment",
    "30-1": "The value for this key is the segment ID. The campaign is created only if the segment is processed. If you add this key, you need not mention the \"where\" clause key.",
    "30-2": "Optional",
    "30-3": "Integer",
    "30-4": "upper_cap_for_target_segment\\*",
    "31-0": "**upper_cap_for_target_segment\\***",
    "31-1": "Do not send the campaign if the target segment exceeds the provided number of users.  \nBy default all target segment users are selected.  \nNote: Do not provide this parameter if users_limit_overall is provided. (Minimum value is 100)",
    "31-2": "Optional",
    "31-3": "Integer",
    "31-4": "\"upper_cap_for_target_segment” : 500",
    "32-0": "**users_limit_overall\\***",
    "32-1": "Send only to the provided number of qualified users.  \nBy default all target segment users are selected.  \nNote: Do not provide this parameter if upper_cap_for_target_segment is provided. (Minimum value is 100)",
    "32-2": "Optional",
    "32-3": "Integer",
    "32-4": "“users_limit_overall” : 250",
    "33-0": "**users_limit_per_run\\***",
    "33-1": "Send only to the provided number of users per run.",
    "33-2": "Optional",
    "33-3": "Integer",
    "33-4": "\"users_limit_per_run\":199,",
    "34-0": "send_email_to_opted_out_users",
    "34-1": "Valid values are: true and false. Set to false by default. When true, emails are sent to all qualified users, including those who have unsubscribed. For more information, refer to [Override Communication Preferences for Email](https://docs.clevertap.com/docs/create-message-email#override-communication-preferences-for-email).",
    "34-2": "Optional",
    "34-3": "Boolean",
    "34-4": "\"send_email_to_opted_out_users\":true",
    "35-0": "content",
    "35-1": "Object that defines the content for your message.",
    "35-2": "Required",
    "35-3": "String",
    "35-4": "\"content\": {  \n\"subject\": \"Welcome\",  \n\"body\": \"  \nYour HTML content for the email  \n\",  \n\"sender_name\": \"CleverTap\"  \n}",
    "36-0": "content.title",
    "36-1": "Title of your push notification message and web push message sent to the user.",
    "36-2": "Required",
    "36-3": "String",
    "36-4": "“Hi”",
    "37-0": "content.body",
    "37-1": "Body content of your push notification, web push, email and sms sent to the user.",
    "37-2": "Required",
    "37-3": "String",
    "37-4": "“Have you seen the special offer?”",
    "38-0": "content.subject",
    "38-1": "Subject of your email sent to the user.",
    "38-2": "Required",
    "38-3": "String",
    "38-4": "“Subject”",
    "39-0": "content.sender_name",
    "39-1": "Sender name for your email sent to the user.",
    "39-2": "Required",
    "39-3": "String",
    "39-4": "“Onboarding”",
    "40-0": "content.template_name",
    "40-1": "Namespace of the WhatsApp template you want to choose. This is available in settings for all the added WhatsApp templates.",
    "40-2": "Required",
    "40-3": "String",
    "40-4": "\"662da022_5583_406d_9c71_428d5e3164bb:ticket_update\"",
    "41-0": "content.replacements",
    "41-1": "Add values that replace the placeholders in the templates.",
    "41-2": "Required",
    "41-3": "String",
    "41-4": "\"replacements\" : {  \n\"1\" : ,  \n\"2\" : ,  \n\"3\" :  \n}",
    "42-0": "content.header",
    "42-1": "Allows you to attach images, videos, documents, and locations in your WhatsApp message based on the attachment type defined in your interactive template.",
    "42-2": "Optional",
    "42-3": "Object",
    "42-4": "",
    "43-0": "content.body",
    "43-1": "Dynamically replaces placeholder values in the body of interactive templates.",
    "43-2": "Required",
    "43-3": "Object",
    "43-4": "",
    "44-0": "content.button",
    "44-1": "Allows you to dynamically add a suffix to CTA buttons with dynamic URL.",
    "44-2": "Optional",
    "44-3": "Object",
    "44-4": "",
    "45-0": "content.locale",
    "45-1": "The language code of the message. To get the language code from the CT dashboard, go to Settings > Channels > Whatsapp and open any template. Click the **Template** tab. The template name displays the locale code in parentheses. For example,  Dynamic URL _(fr)_.",
    "45-2": "Required",
    "45-3": "String",
    "45-4": "\"fr\" for French or \"de\" for German.",
    "46-0": "wzrk_cid",
    "46-1": "Platform-specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.  \nIf a Notification ID is mandatory, then the channel ID is expected. An error occurs if a valid channel ID is not received.  \n  \n**Note:** If the channel ID is not saved on the CleverTap dashboard, then the channel ID does not work in Create Campaign API.",
    "46-2": "Required",
    "46-3": "String",
    "46-4": "",
    "47-0": "wzrk_bc",
    "47-1": "Platform-specific key that can be used when targeting users on devices running Android O - can be used to determine the notification count shown on the app icon",
    "47-2": "Optional",
    "47-3": "Int",
    "47-4": "",
    "48-0": "wqrk_bi",
    "48-1": "Platform-specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.  \nAccepted values:  \napp icon  \nnotification icon",
    "48-2": "Optional",
    "48-3": "String",
    "48-4": "",
    "49-0": "wzrk_acts",
    "49-1": "Android Action Buttons  \n\"l\"= Action Title, \"dl\" = Deep link, \"id\" = Action id, \"ico\" = icon resource identifier",
    "49-2": "Optional",
    "49-3": "Object",
    "49-4": "wzrk_acts: \"eters]  \n{  \n  \"data\": {  \n    \"h-0\": \"Parameter\",  \n    \"h-1",
    "50-0": "platform_specific",
    "50-1": "The platform-specific details for Push and Web Push notification",
    "50-2": "Optional",
    "50-3": "String",
    "50-4": "",
    "51-0": "enable_rendermax",
    "51-1": "This field helps enable RenderMax when creating a campaign via API. It is available only for Mobile Push campaigns and must be added under the platform_specific key for Android devices only. For more information about the feature, refer to [RenderMax](https://docs.clevertap.com/docs/rendermax). Valid values are: true or false.",
    "51-2": "Optional",
    "51-3": "Boolean",
    "51-4": "\"enable_rendermax\": true",
    "52-0": "background_image",
    "52-1": "The parameter to render images in the push notification for Android.",
    "52-2": "Optional",
    "52-3": "String",
    "52-4": "\"background_image\": \"<http://example.jpg>\"",
    "53-0": "**notification_tray_priority\\***",
    "53-1": "This field helps to set priority for the notification tray in android.  \nValid values are:  \n\"max\"  \n\"high\"  \n\"default\"",
    "53-2": "Optional",
    "53-3": "String",
    "53-4": "\"notification_tray_priority\": \"max\"",
    "54-0": "**delivery_priority\\***",
    "54-1": "This field helps to set delivery priority of notification in android.  \nValid values are:  \n\"normal\"  \n\"high\"",
    "54-2": "Optional",
    "54-3": "String",
    "54-4": "\"delivery_priority\": \"high\"",
    "55-0": "**android_collapse\\***",
    "55-1": "User can send any string value or number to identify collapsible notification in android.",
    "55-2": "Optional",
    "55-3": "String/Integer",
    "55-4": "\"android_collapse\" : \"any message or number\".",
    "56-0": "**ios_collapse\\***",
    "56-1": "User can send any string value or number to identify collapsible notification in iOS.",
    "56-2": "Optional",
    "56-3": "String/Integer",
    "56-4": "\"ios_collapse\" : \"any message or number\"",
    "57-0": "**summary\\***",
    "57-1": "You can provide summary  \nabout the notification you are sending to user valid only in Android.",
    "57-2": "Optional",
    "57-3": "String",
    "57-4": "\"summary\": \"Enter summary here\"",
    "58-0": "**subtitle\\***",
    "58-1": "Subtitle for your notification.",
    "58-2": "Optional",
    "58-3": "String",
    "58-4": "\"subtitle\": \"Enter subtitle\"",
    "59-0": "**large_icon\\***",
    "59-1": "By default, the app icon appears as the large icon. The CleverTap dashboard provides an option to show other icons apart from the app icon.          ",
    "59-2": "Optional",
    "59-3": "String (URL)",
    "59-4": "\"large_icon\": \"logo.com\"",
    "60-0": "**Custom KVs\\***",
    "60-1": "You can specify it in the platform specific object.",
    "60-2": "Optional",
    "60-3": "key-value pair",
    "60-4": "\"platform_specific\": {  \n\"ios\": {  \n\"Key\": \"Value_ios\"  \n},  \n\"android\": {  \n\"Key\": \"Value_android\",  \n}  \n}",
    "61-0": "mutable-content",
    "61-1": "To raise Notification viewed event for Push notifications, raise the flag to \"true\" in the platform_specific: iOS section of the request payload.",
    "61-2": "Optional",
    "61-3": "Boolean",
    "61-4": "",
    "62-0": "respect_frequency_caps",
    "62-1": "Set to false if you want to override frequency caps and dwell time.",
    "62-2": "Optional",
    "62-3": "Boolean",
    "62-4": "TRUE",
    "63-0": "respect_throttle",
    "63-1": "Toggle this flag to true if you want to enable throttle limits for the campaign.",
    "63-2": "Optional",
    "63-3": "Boolean",
    "63-4": "\"respect_throttle\":true",
    "64-0": "ttl",
    "64-1": "This allows setting of time to live for push notifications for android and iOS.  \nThe subkeys for ttl are  \n“ttl_type” - with input as “relative”   \"h-1\":  and ”absolute”  \n“value” -For relative ttl, the input is an integer in minutes.  \nFor absolute ttl, the input should be an integer in the yyyyMMddHHmm format.  \n  \nMore information is available [here](https://docs.clevertap.com/docs/create-message-push).",
    "64-2": "Optional",
    "64-3": "Depends on subkey  \nttl_type: string  \nvalue: Long",
    "64-4": "For Absolute TTL -  \n\"ttl\" : {  \n\"ttl_type\": \"absolute\",  \n\"value\" : 202207200000  \n},  \nFor Relative TTL -  \n\"ttl\" : {  \n\"ttl_type\": \"relative\",  \n\"value\" : 2880  \n},",
    "65-0": "when",
    "65-1": "When you want to send out the messages. Valid inputs are now (to send the notification right away) or YYYYMMDD HH:MM to schedule the messages for a specific date and time in the future.",
    "65-2": "Required",
    "65-3": "String",
    "65-4": "\"now\"",
    "66-0": "**when\\***",
    "66-1": "Allows you to set Date, time and Delivery preferences of the message.",
    "66-2": "Required",
    "66-3": "Object",
    "66-4": "\"when\":{  \n\"type\":\"later\",  \n\"delivery_date_time\": [\"20230503 15:20\"],  \n\"h-1\": \"Description\",  \n\"delivery_timezone\":\"user\",  \n\"user_timezone_wrap_around\":true,  \n\"dnd\":{  \n\"message_state\":1,  \n\"dnd_timezone\":\"user\",  \n\"dnd_info\":{  \n\"2\": 0-1\": \"The name o\",  \n\"4\": 0-2\": \"Required\",  \n}  \n},  \n\"campaign_cutoff\": \"14:20\"  \n}",
    "67-0": "**when.type\\***",
    "67-1": "Valid inputs are \"now\" or \"later\" (to send it at a later time) or \"recurring\".",
    "67-2": "Required",
    "67-3": "String",
    "67-4": "\"type\" : \"later\"",
    "68-0": "**when.delivery_date_time\\***",
    "68-1": "\"YYYYMMDD HH:MM\" to schedule the messages for a specific date and time in the future in increasing date order only.",
    "68-2": "Required if when.type = \"later\"",
    "68-3": "String",
    "68-4": "\"delivery_date_time\": [\"20230503 15:20\"]",
    "69-0": "**when.repeats_every\\***",
    "69-1": "Number of days or weeks to be repeated on.",
    "69-2": "Required if when.type = \"recurring\"",
    "69-3": "Integer",
    "69-4": "\"repeats_every\" : 2",
    "70-0": "**when.repeat_type\\***",
    "70-1": "Can be \"day\" or \"week\"  \nNote: Repeat by month is not implemented.",
    "70-2": "Required if when.type = \"recurring\"",
    "70-3": "String",
    "70-4": "\"repeat_type\" : \"week\"",
    "71-0": "**when.start_time\\***",
    "71-1": "\"YYYYMMDD HH:MM\" (Cannot provide past date).",
    "71-2": "Required if when.type = \"recurring\"",
    "71-3": "String",
    "71-4": "\"start_time\" : \"20220415 12:05\",",
    "72-0": "**when.end_by_date\\***",
    "72-1": "\"YYYYMMDD\", End the campaign after provided date.  \nNote: Either end_by_date or end_by_occurrences can be provided.",
    "72-2": "Optional  \nNote: This is applicable only if when.type = \"recurring\"",
    "72-3": "String",
    "72-4": "\"end_by_date\" : \"20220420\"",
    "73-0": "**when.end_by_occurrences\\***",
    "73-1": "End the campaign after provided number of occurrences.  \nNote: Either end_by_date or end_by_occurrences can be provided.",
    "73-2": "Note: This is applicable optional only if when.type = \"recurring\"",
    "73-3": "Integer",
    "73-4": "\"end_by_occurrences\" : 8",
    "74-0": "**when.repeat_on_days_of_week\\***",
    "74-1": "Days of week represented as \": {  \n for h-0\": \"Parameter\".",
    "74-2": "Required if when.type = \"recurring\" and repeat_type = \"week\"",
    "74-3": "List",
    "74-4": "\"repeat_on_days_of_week\" : a\": {",
    "75-0": "**when.delivery_timezone\\***",
    "75-1": "To adapt delivery times to user or account's timezone.",
    "75-2": "Optional",
    "75-3": "Boolean",
    "75-4": "\"delivery_timezone\":\"user\"  \nOR  \n\"delivery_timezone\":\"account\"",
    "76-0": "**when.user_timezone_wrap_around\\***",
    "76-1": "If the campaign start time is after a user's local time, then drop the campaign (user_timezone_wrap_around : false) or deliver the campaign the next day (user_timezone_wrap_around : true).",
    "76-2": "Optional",
    "76-3": "Boolean",
    "76-4": "\"user_timezone_wrap_around\":true",
    "77-0": "**dnd\\***",
    "77-1": "Do Not Disturb",
    "77-2": "Optional",
    "77-3": "Object",
    "77-4": "\"dnd\":{  \n\"message_state\":\"delay\",  \n\"dnd_timezone\":\"user\",  \n\"dnd_info\":{  \n\"2\": h-1\": \"Description\",  \n\"4\": h-2\": \"Required/O\"  \n}  \n},",
    "78-0": "**dnd.message_state\\***",
    "78-1": "The following are the values for parameter:  \n\"delay\": Delay message send until the end of DND.  \n\"discard\" : Discard messages scheduled during DND.  \nNote: By default, messages scheduled during DND are discarded (if message_state is not provided).",
    "78-2": "Required if DND applicable.",
    "78-3": "Integer",
    "78-4": "\"message_state\":\"discard\"",
    "79-0": "**dnd.dnd_info\\***",
    "79-1": "Provides the following information:  \nFormat: a\": {  \n    \"h-0\": \"Parameter\",  \n    \"h-1\": \"De  \nDay: Denotes the days of week on which the DND is applicable.  \nFormat: \": \"Paramete is     \"h-1 value",
    "79-2": "Required if DND applicable",
    "79-3": "Object",
    "79-4": "\"dnd_info\":{  \n\"2\": data\": {  \n    \"h-0,  \n\"4\": ta\": {  \n    \"h-0\":  \n}  \n(2 for monday)",
    "80-0": "**dnd.dnd_timezone\\***",
    "80-1": "Timezone for DND (user_timezone or account_timezone)",
    "80-2": "Required if DND applicable",
    "80-3": "String",
    "80-4": "\"dnd_timezone\":\"user\",  \nOR  \n\"dnd_timezone\":\"account\",",
    "81-0": "**when.campaign_cutoff\\***",
    "81-1": "Stop sending messages from this campaign after a particular time (Campaign Cutoff)  \nFormat : \"HH:MM\"",
    "81-2": "Optional",
    "81-3": "String (Time)",
    "81-4": "\"campaign_cutoff\": \"12:20\""
  },
  "cols": 5,
  "rows": 82,
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
    "provider_nick_name": "Provider_1",
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
        "locale": "en",
        "replacements": {
            "1": "buddy",
            "2":"Где ты се, Pад познакомиться."
        }
    },
    "when": "now"
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
                "latitiude": 32.123,
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
            "latitiude":32.123,
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

## Example Request - Target User Events & Properties

Here is an example cURL request to the _Create Campaign_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d ' {"name": "My Sms API campaign","estimate_only": true,"target_mode":"sms", "where":{"event_name":"Charged","from":20171001,"to":20171220,"common_profile_properties":{"profile_fields": [ {"name": "Customer Type","operator":"equals","value": "Platinum"}]}},"respect_frequency_caps": false,"content": { "body": "Sms body"},"when": "now"}' \
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
    "0-4": "\"to\": {  \n        \"FBID\":   \"h-0\": \"Parameter\",  \n    \"h-1\",  \n  \n```\n    \"Email\": ,\n\"h-4\": \"Example Value\",\n\"0-0\": \"to\",\n\"0-1\",\n    \"Identity\": \",\n\"0-3\": \"object\",\n\"0-4\",\n    \"objectId\": ,\n\"1-1\": \"Facebook ID for user.\",\n\"1-2\": \"required\",\n\"1-3\"\n}\n```",
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
    "5-1": "Each message can be tagged into a tag group. Metrics, such as sent and viewed, will be shown against each tag group in the CleverTap dashboard. Max 500 groups. If not specified, the metrics will be shown against the “Not Tagged” tag. The stats page shows the tag group for the selected tag.",
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
    "12-4": " \"template\": {  \n            \"template_name\": \"Image_Template_1\",  \n              \"header\": {  \n                \"type\": \"image\",  \n                \"media\": {  \n                    \"url\": \"<https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo_1280x720_5mb.jpg\">  \n                }  \n            },  \n            \"body\": {  \n                \"type\": \"text\",  \n                \"replacements\": [\"var1\",\"Var2\"]  \n            },  \n  \n```\n        \"buttons\": \\[\n            {\n                \"replacements\": [  \n                    \"mvi qr payload 5\"  \n                ]\n            },\n            {\n                \"replacements\": [  \n                    \"mvi qr payload 7\"  \n                ]\n            }\n        ]\n```}",
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
    "18-4": "\"header\": {  \n                \"type\": \"text\",  \n                \"replacements\": [ \"header var 1\"   ]  \n  \n```\n        },\n```",
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
    "23-1": "Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is required for WhatsApp.",
    "23-2": "Optional (required for WhatsApp)",
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
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
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

A success response implies payload validity and addition to the processing queue. Message delivery is handled by the provider (push or email) and depends on if you provided a valid recipient id.

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
    "16-2": "The number of variables and values in the payload must match for body, header, or button. "
  },
  "cols": 3,
  "rows": 17,
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

If you’re using Amazon SES or your own SMTP gateway, you’ll have to follow the steps [listed on this page](https://docs.clevertap.com/docs/handling-unsubscribes) to enable your users to unsubscribe from your email notifications.

# FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
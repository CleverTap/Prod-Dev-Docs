---
title: Create Campaign
excerpt: >-
  Learn how to use the Create Campaign API to send targeted messages to your
  users based on events and properties or user identities.
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: index
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
[https://in1.api.clevertap.com/1/targets/create.json](https://in1.api.clevertap.com/1/targets/create.json)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method - Target User Events & Properties

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters - Target User Events & Properties

> 📘 Mobile Push Notification Parameters
>
> The parameters in bold and marked with * provide additional functionality for creating a push campaign.

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
        Required/Optional
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
        The name of your campaign shown in the CleverTap dashboard.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "My Campaign"
      </td>
    </tr>

    <tr>
      <td>
        target_mode
      </td>

      <td>
        Allows you to specify the type of campaign being created, such as push notifications, web push, email, etc.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "Push"
      </td>
    </tr>

    <tr>
      <td>
        teams
      </td>

      <td>
        Allows you to specify the team to which a campaign belongs. Each campaign can belong to only one team. Team names must match names configured in your account and are case-sensitive. For more information, refer to [Teams](https://docs.clevertap.com/docs/teams-setup).
      </td>

      <td>
        Required if Teams is enabled
      </td>

      <td>
        List of strings
      </td>

      <td>
        "Team 1"
      </td>
    </tr>

    <tr>
      <td>
        provider_nick_name
      </td>

      <td>
        Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is required for WhatsApp and SMS.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "Provider 1"
      </td>
    </tr>

    <tr>
      <td>
        provider_group_nickname
      </td>

      <td>
        Allows you to specify which provider group to use for your SMS campaign. You must use the name as added in your account settings
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "NewTestSMSProviderGroup"
      </td>
    </tr>

    <tr>
      <td>
        content.sms.template_id
      </td>

      <td>
        Template ID (required for sending SMS to phone numbers of India)
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal***
      </td>

      <td>
        Checks the end goal of the campaign for conversion tracking.
      </td>

      <td>
        Optional
      </td>

      <td>
        Object
      </td>

      <td>
        "conversion_goal":{'{'}  
        "event_name": "Charged",  
        "filter_type":{'{'}  
        "event_property":[  
        {'{'}  
        "property_name":"Items|Book name",  
        "operator":"equals",  
        "property_value":"book name"  
        {'}'},  
        {'{'}  
        "property_name":"Items|Category",  
        "operator":"equals",  
        "property_value":"category"  
        {'}'},  
        {'{'}  
        "property_name":"Payment mode",  
        "operator":"equals",  
        "property_value":"UPI"  
        {'}'},  
        {'{'}  
        "property_name":"Amount",  
        "operator":"between",  
        "property_value":[100, 500]  
        {'}'}  
        ],  
        "first_time":"yes",  
        "last_time":"yes",  
        "time_of_day":[["21:00","23:00"]],  
        "day_of_week":[1,2],  
        "day_of_month":[1,15]  
        {'}'},  
        "conversion_time":"3D",  
        "revenue_property": "Amount"\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.event_name***
      </td>

      <td>
        Name of the goal you want to track.
      </td>

      <td>
        Required if conversion_goal is provided
      </td>

      <td>
        String
      </td>

      <td>
        "event_name": "Charged"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type***
      </td>

      <td>
        Filters for provided event_name  
        The type of filter applied in conversion tracking. For example, first_time, time_of_day and so on.
      </td>

      <td>
        Required if conversion_goal is provided
      </td>

      <td>
        Object
      </td>

      <td>
        "filter_type":{'{'}  
        "event_property": [],  
        "first_time":"yes",  
        "last_time":"yes",  
        "time_of_day":[],  
        "day_of_week":[],  
        "day_of_month": []\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.event_property***
      </td>

      <td>
        Event Property for given event_name.
      </td>

      <td>
        Optional
      </td>

      <td>
        List
      </td>

      <td>
        "event_property":[  
        {'{'}  
        "property_name":"Items|Book name",  
        "operator":"equals",  
        "property_value":"book name"  
        {'}'}  
        ]
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.event_property.property_name***
      </td>

      <td>
        Name of the event_property.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "property_name":"Items|Book name"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.event_property.operator***
      </td>

      <td>
        Operator required for property_name and property_value.  
        Valid values are:  
        equals  
        greater_than  
        greater_than_equals  
        less_than  
        less_than_equals  
        contains  
        does_not_contain  
        not_equals  
        exists (does not require property_value)  
        does_not_exist (does not require property_value)  
        between (requires 2 values in an array)
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "operator":"equals"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.event_property.property_value***
      </td>

      <td>
        Provides value of the event_property.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "property_value":"book name"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.first_time***
      </td>

      <td>
        Provides value of the first_time.  
        Valid values are:  
        "yes"  
        "no"
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "first_time":"yes"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.last_time***
      </td>

      <td>
        Provides value of last_time.  
        Valid values:  
        "yes"  
        "no"
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "last_time":"yes"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.time_of_day***
      </td>

      <td>
        Provides value of time_of_day.  
        Valid values:  
        Array of time ranges  
        Format: "HH:MM"
      </td>

      <td>
        Optional
      </td>

      <td>
        List of lists
      </td>

      <td>
        "time_of_day":[["21:00","23:00"],["11:34","12:44"]]
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.day_of_week***
      </td>

      <td>
        Provides value of day_of_week.  
        Valid values: 1-7 (1=Monday, 7=Sunday)
      </td>

      <td>
        Optional
      </td>

      <td>
        List
      </td>

      <td>
        "day_of_week":[1,7]
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.filter_type.day_of_month***
      </td>

      <td>
        Provides value of day_of_month.  
        Valid values: 1-31
      </td>

      <td>
        Optional
      </td>

      <td>
        List
      </td>

      <td>
        "day_of_month":[1,15,31]
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.conversion_time***
      </td>

      <td>
        Denotes the period that conversions are tracked for once the campaign is published.  
        Accepted values: Numbers followed by m (minutes), H (hour), D (day), W (week), M (month)  
        where, m: minutes, H: hour, D: day, W: week, M:month
      </td>

      <td>
        Required if conversion_goal is provided
      </td>

      <td>
        String
      </td>

      <td>
        "conversion_time":"3D"
      </td>
    </tr>

    <tr>
      <td>
        **conversion_goal.revenue_property***
      </td>

      <td>
        Denotes the revenue property for conversion goal event. For example, amount, currency, payment mode and so on.
      </td>

      <td>
        Required if conversion_goal is provided
      </td>

      <td>
        String
      </td>

      <td>
        "revenue_property": "Amount"
      </td>
    </tr>

    <tr>
      <td>
        where
      </td>

      <td>
        Allows you to filter target base on the user events and profile properties. Send an empty object ({'{}{}'}) to target your entire user base.
      </td>

      <td>
        Required
      </td>

      <td>
        Object
      </td>

      <td>
        "where": {'{'}  
        "event_name": "Charged",  
        "from": 20171001,  
        "to": 20171220,  
        "common_profile_properties": {'{'}  
        "profile_fields": [  
        {'{'}  
        "name": "Customer Type",  
        "operator":"equals",  
        "value": "Platinum"  
        {'}'}  
        ]  
        {'}'}\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        where.event_name
      </td>

      <td>
        Allows you to target users based on an event they perform.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
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
        Optional
      </td>

      <td>
        Int
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
        Optional
      </td>

      <td>
        Int
      </td>

      <td>
        20171220
      </td>
    </tr>

    <tr>
      <td>
        where.profile_fields
      </td>

      <td>
        Allows you to target users based on the values of their profile fields.
      </td>

      <td>
        Optional
      </td>

      <td>
        Array
      </td>

      <td>
        "common_profile_properties": {'{'}  
        "profile_fields": [  
        {'{'}  
        "name": "Customer Type",  
        "operator":"equals",  
        "value": "Platinum"  
        {'}'}  
        ]\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        estimate_only
      </td>

      <td>
        If this parameter is set to true, the request returns an estimated reach of the campaign, which is the number of users who receive the notification when you send it out. Setting this parameter to true does not create the campaign.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        FALSE
      </td>
    </tr>

    <tr>
      <td>
        async_estimate
      </td>

      <td>
        This allows setting of the estimate_only flag asynchronously. If this parameter is set to true, it returns a request ID that can be passed in the subsequent API call.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        Boolean
      </td>
    </tr>

    <tr>
      <td>
        system_control_group_include
      </td>

      <td>
        If the system control group is enabled from the dashboard, you can choose to use the control group for API campaigns.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        control_group
      </td>

      <td>
        If the custom control group is enabled from the dashboard, you can choose to use the control group for API campaigns
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "control_group": {'{'}  
        "type": "custom",  
        "name": "name of control group"\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        control_group
      </td>

      <td>
        You can create a campaign control group via API by specifying the % as described on the left
      </td>

      <td>
        Optional
      </td>

      <td>
        Integer
      </td>

      <td>
        "control_group": {'{'}  
        "type": "campaign",  
        "percentage": 5\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        skip_estimate
      </td>

      <td>
        Toggle this flag to true if you do not want to estimate the reach. This will lead to faster campaign delivery.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        "skip_estimate":true
      </td>
    </tr>

    <tr>
      <td>
        send_to_all_devices
      </td>

      <td>
        The flag is set to false by default. If no value is specified, then the notification is sent to the last active device. Toggle this flag to true if you want to send notification to all active devices.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        FALSE
      </td>
    </tr>

    <tr>
      <td>
        segment
      </td>

      <td>
        The value for this key is the segment ID. The campaign is created only if the segment is processed. If you add this key, you need not mention the "where" clause key.
      </td>

      <td>
        Optional
      </td>

      <td>
        Integer
      </td>

      <td>
        12345
      </td>
    </tr>

    <tr>
      <td>
        **upper_cap_for_target_segment***
      </td>

      <td>
        Do not send the campaign if the target segment exceeds the provided number of users.  
        By default all target segment users are selected.  
        Note: Do not provide this parameter if users_limit_overall is provided. (Minimum value is 100)
      </td>

      <td>
        Optional
      </td>

      <td>
        Integer
      </td>

      <td>
        "upper_cap_for_target_segment" : 500
      </td>
    </tr>

    <tr>
      <td>
        **users_limit_overall***
      </td>

      <td>
        Send only to the provided number of qualified users.  
        By default all target segment users are selected.  
        Note: Do not provide this parameter if upper_cap_for_target_segment is provided. (Minimum value is 100)
      </td>

      <td>
        Optional
      </td>

      <td>
        Integer
      </td>

      <td>
        "users_limit_overall" : 250
      </td>
    </tr>

    <tr>
      <td>
        **users_limit_per_run***
      </td>

      <td>
        Send only to the provided number of users per run.
      </td>

      <td>
        Optional
      </td>

      <td>
        Integer
      </td>

      <td>
        "users_limit_per_run":199
      </td>
    </tr>

    <tr>
      <td>
        send_email_to_opted_out_users
      </td>

      <td>
        Valid values are: true and false. Set to false by default. When true, emails are sent to all qualified users, including those who have unsubscribed. For more information, refer to [Override Communication Preferences for Email](https://docs.clevertap.com/docs/create-message-email#override-communication-preferences-for-email).
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        "send_email_to_opted_out_users":true
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
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "content": {'{'}  
        "subject": "Welcome",  
        "body": "Your HTML content for the email",  
        "sender_name": "CleverTap"\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        content.title
      </td>

      <td>
        Title of your push notification message and web push message sent to the user.
      </td>

      <td>
        Required
      </td>

      <td>
        String
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
        Body content of your push notification, web push, email and sms sent to the user.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "Have you seen the special offer?"
      </td>
    </tr>

    <tr>
      <td>
        content.subject
      </td>

      <td>
        Subject of your email sent to the user.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "Subject"
      </td>
    </tr>

    <tr>
      <td>
        content.sender_name
      </td>

      <td>
        Sender name for your email sent to the user.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "Onboarding"
      </td>
    </tr>

    <tr>
      <td>
        content.template_name
      </td>

      <td>
        Namespace of the WhatsApp template you want to choose. This is available in settings for all the added WhatsApp templates.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "662da022_5583_406d_9c71_428d5e3164bb:ticket_update"
      </td>
    </tr>

    <tr>
      <td>
        content.replacements
      </td>

      <td>
        Add values that replace the placeholders in the templates.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "replacements" : {'{'}  
        "1" : "value1",  
        "2" : "value2",  
        "3" : "value3"\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        content.header
      </td>

      <td>
        Allows you to attach images, videos, documents, and locations in your WhatsApp message based on the attachment type defined in your interactive template.
      </td>

      <td>
        Optional
      </td>

      <td>
        Object
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        content.body
      </td>

      <td>
        Dynamically replaces placeholder values in the body of interactive templates.
      </td>

      <td>
        Required
      </td>

      <td>
        Object
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        content.button
      </td>

      <td>
        Allows you to dynamically add a suffix to CTA buttons with dynamic URL.
      </td>

      <td>
        Optional
      </td>

      <td>
        Object
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        content.locale
      </td>

      <td>
        The language code of the message. To get the language code from the CT dashboard, go to Settings {'>'} Channels {'>'} Whatsapp and open any template. Click the **Template** tab. The template name displays the locale code in parentheses. For example,  Dynamic URL _(fr)_.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "fr" for French or "de" for German.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_cid
      </td>

      <td>
        Platform-specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.  
        If a Notification ID is mandatory, then the channel ID is expected. An error occurs if a valid channel ID is not received.

        **Note:** If the channel ID is not saved on the CleverTap dashboard, then the channel ID does not work in Create Campaign API.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        wzrk_bc
      </td>

      <td>
        Platform-specific key that can be used when targeting users on devices running Android O - can be used to determine the notification count shown on the app icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Int
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        wqrk_bi
      </td>

      <td>
        Platform-specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.  
        Accepted values:  
        app icon  
        notification icon
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        wzrk_acts
      </td>

      <td>
        Android Action Buttons  
        "l"= Action Title, "dl" = Deep link, "id" = Action id, "ico" = icon resource identifier
      </td>

      <td>
        Optional
      </td>

      <td>
        Object
      </td>

      <td>
        "wzrk_acts": [  
        {'{'}  
        "l": "Action button 1",  
        "dl": "deep link for action button 1",  
        "id": "action id of the action button 1",  
        "ico": "Icon resource identifier for action button 1"  
        {'}'}  
        ]
      </td>
    </tr>

    <tr>
      <td>
        platform_specific
      </td>

      <td>
        The platform-specific details for Push and Web Push notification
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        enable_rendermax
      </td>

      <td>
        This field helps enable RenderMax when creating a campaign via API. It is available only for Mobile Push campaigns and must be added under the platform_specific key for Android devices only. For more information about the feature, refer to [RenderMax](https://docs.clevertap.com/docs/rendermax). Valid values are: true or false.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        "enable_rendermax": true
      </td>
    </tr>

    <tr>
      <td>
        background_image
      </td>

      <td>
        The parameter to render images in the push notification for Android.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "background_image": "[http://example.jpg](http://example.jpg)"
      </td>
    </tr>

    <tr>
      <td>
        **notification_tray_priority***
      </td>

      <td>
        This field helps to set priority for the notification tray in android.  
        Valid values are:  
        "max"  
        "high"  
        "default"
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "notification_tray_priority": "max"
      </td>
    </tr>

    <tr>
      <td>
        **delivery_priority***
      </td>

      <td>
        This field helps to set delivery priority of notification in android.  
        Valid values are:  
        "normal"  
        "high"
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "delivery_priority": "high"
      </td>
    </tr>

    <tr>
      <td>
        **android_collapse***
      </td>

      <td>
        User can send any string value or number to identify collapsible notification in android.
      </td>

      <td>
        Optional
      </td>

      <td>
        String/Integer
      </td>

      <td>
        "android_collapse" : "any message or number"
      </td>
    </tr>

    <tr>
      <td>
        **ios_collapse***
      </td>

      <td>
        User can send any string value or number to identify collapsible notification in iOS.
      </td>

      <td>
        Optional
      </td>

      <td>
        String/Integer
      </td>

      <td>
        "ios_collapse" : "any message or number"
      </td>
    </tr>

    <tr>
      <td>
        **summary***
      </td>

      <td>
        You can provide summary about the notification you are sending to user valid only in Android.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "summary": "Enter summary here"
      </td>
    </tr>

    <tr>
      <td>
        **subtitle***
      </td>

      <td>
        Subtitle for your notification.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "subtitle": "Enter subtitle"
      </td>
    </tr>

    <tr>
      <td>
        **large_icon***
      </td>

      <td>
        By default, the app icon appears as the large icon. The CleverTap dashboard provides an option to show other icons apart from the app icon.
      </td>

      <td>
        Optional
      </td>

      <td>
        String (URL)
      </td>

      <td>
        "large_icon": "logo.com"
      </td>
    </tr>

    <tr>
      <td>
        **Custom KVs***
      </td>

      <td>
        You can specify it in the platform specific object.
      </td>

      <td>
        Optional
      </td>

      <td>
        key-value pair
      </td>

      <td>
        "platform_specific": {'{'}  
        "ios": {'{'}  
        "Key": "Value_ios"  
        {'}'},  
        "android": {'{'}  
        "Key": "Value_android"  
        {'}'}\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        mutable-content
      </td>

      <td>
        To raise Notification viewed event for Push notifications, raise the flag to "true" in the platform_specific: iOS section of the request payload.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        respect_frequency_caps
      </td>

      <td>
        Set to false if you want to override frequency caps and dwell time.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        TRUE
      </td>
    </tr>

    <tr>
      <td>
        respect_throttle
      </td>

      <td>
        Toggle this flag to true if you want to enable throttle limits for the campaign.
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        "respect_throttle":true
      </td>
    </tr>

    <tr>
      <td>
        ttl
      </td>

      <td>
        This allows setting of time to live for push notifications for android and iOS.  
        The subkeys for ttl are  
        "ttl_type" - with input as "relative" and "absolute"  
        "value" - For relative ttl, the input is an integer in minutes.  
        For absolute ttl, the input should be an integer in the yyyyMMddHHmm format.

        More information is available [here](https://docs.clevertap.com/docs/create-message-push).
      </td>

      <td>
        Optional
      </td>

      <td>
        Depends on subkey  
        ttl_type: string  
        value: Long
      </td>

      <td>
        For Absolute TTL -  
        "ttl" : {'{'}  
        "ttl_type": "absolute",  
        "value" : 202207200000  
        {'}'},   
        For Relative TTL -  
        "ttl" : {'{'}  
        "ttl_type": "relative",  
        "value" : 2880\

        {'}'}
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
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "now"
      </td>
    </tr>

    <tr>
      <td>
        **when***
      </td>

      <td>
        Allows you to set Date, time and Delivery preferences of the message.
      </td>

      <td>
        Required
      </td>

      <td>
        Object
      </td>

      <td>
        "when":{'{'}  
        "type":"later",  
        "delivery_date_time": ["20230503 15:20"],  
        "delivery_timezone":"user",  
        "user_timezone_wrap_around":true,  
        "dnd":{'{'}  
        "message_state":"delay",  
        "dnd_timezone":"user",  
        "dnd_info":{'{'}  
        "2": ["21:00","06:00"],  
        "4": ["23:56","04:34"]  
        {'}'}  
        {'}'},  
        "campaign_cutoff": "14:20"\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        **when.type***
      </td>

      <td>
        Valid inputs are "now" or "later" (to send it at a later time) or "recurring".
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "type" : "later"
      </td>
    </tr>

    <tr>
      <td>
        **when.delivery_date_time***
      </td>

      <td>
        "YYYYMMDD HH:MM" to schedule the messages for a specific date and time in the future in increasing date order only.
      </td>

      <td>
        Required if when.type = "later"
      </td>

      <td>
        String
      </td>

      <td>
        "delivery_date_time": ["20230503 15:20"]
      </td>
    </tr>

    <tr>
      <td>
        **when.repeats_every***
      </td>

      <td>
        Number of days or weeks to be repeated on.
      </td>

      <td>
        Required if when.type = "recurring"
      </td>

      <td>
        Integer
      </td>

      <td>
        "repeats_every" : 2
      </td>
    </tr>

    <tr>
      <td>
        **when.repeat_type***
      </td>

      <td>
        Can be "day" or "week"  
        Note: Repeat by month is not implemented.
      </td>

      <td>
        Required if when.type = "recurring"
      </td>

      <td>
        String
      </td>

      <td>
        "repeat_type" : "week"
      </td>
    </tr>

    <tr>
      <td>
        **when.start_time***
      </td>

      <td>
        "YYYYMMDD HH:MM" (Cannot provide past date).
      </td>

      <td>
        Required if when.type = "recurring"
      </td>

      <td>
        String
      </td>

      <td>
        "start_time" : "20220415 12:05"
      </td>
    </tr>

    <tr>
      <td>
        **when.end_by_date***
      </td>

      <td>
        "YYYYMMDD", End the campaign after provided date.  
        Note: Either end_by_date or end_by_occurrences can be provided.
      </td>

      <td>
        Optional  
        Note: This is applicable only if when.type = "recurring"
      </td>

      <td>
        String
      </td>

      <td>
        "end_by_date" : "20220420"
      </td>
    </tr>

    <tr>
      <td>
        **when.end_by_occurrences***
      </td>

      <td>
        End the campaign after provided number of occurrences.  
        Note: Either end_by_date or end_by_occurrences can be provided.
      </td>

      <td>
        Optional only if when.type = "recurring"
      </td>

      <td>
        Integer
      </td>

      <td>
        "end_by_occurrences" : 8
      </td>
    </tr>

    <tr>
      <td>
        **when.repeat_on_days_of_week***
      </td>

      <td>
        Days of week represented as numbers (1=Monday, 7=Sunday).
      </td>

      <td>
        Required if when.type = "recurring" and repeat_type = "week"
      </td>

      <td>
        List
      </td>

      <td>
        "repeat_on_days_of_week" : [1,3,5]
      </td>
    </tr>

    <tr>
      <td>
        **when.delivery_timezone***
      </td>

      <td>
        To adapt delivery times to user or account's timezone.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "delivery_timezone":"user"  
        OR  
        "delivery_timezone":"account"
      </td>
    </tr>

    <tr>
      <td>
        **when.user_timezone_wrap_around***
      </td>

      <td>
        If the campaign start time is after a user's local time, then drop the campaign (user_timezone_wrap_around : false) or deliver the campaign the next day (user_timezone_wrap_around : true).
      </td>

      <td>
        Optional
      </td>

      <td>
        Boolean
      </td>

      <td>
        "user_timezone_wrap_around":true
      </td>
    </tr>

    <tr>
      <td>
        **dnd***
      </td>

      <td>
        Do Not Disturb
      </td>

      <td>
        Optional
      </td>

      <td>
        Object
      </td>

      <td>
        "dnd":{'{'}  
        "message_state":"delay",  
        "dnd_timezone":"user",  
        "dnd_info":{'{'}  
        "2": ["21:00","06:00"],  
        "4": ["23:56","04:34"]  
        {'}'}\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        **dnd.message_state***
      </td>

      <td>
        The following are the values for parameter:  
        "delay": Delay message send until the end of DND.  
        "discard" : Discard messages scheduled during DND.  
        Note: By default, messages scheduled during DND are discarded (if message_state is not provided).
      </td>

      <td>
        Required if DND applicable.
      </td>

      <td>
        String
      </td>

      <td>
        "message_state":"discard"
      </td>
    </tr>

    <tr>
      <td>
        **dnd.dnd_info***
      </td>

      <td>
        Provides the following information:  
        Day: Denotes the days of week on which the DND is applicable.  
        Format: Day number (1=Monday, 7=Sunday): ["start_time", "end_time"]
      </td>

      <td>
        Required if DND applicable
      </td>

      <td>
        Object
      </td>

      <td>
        "dnd_info":{'{'}  
        "2": ["21:00","06:00"],  
        "4": ["23:56","04:34"]  
        {'}'}  
        (2 for Tuesday, 4 for Thursday)
      </td>
    </tr>

    <tr>
      <td>
        **dnd.dnd_timezone***
      </td>

      <td>
        Timezone for DND (user_timezone or account_timezone)
      </td>

      <td>
        Required if DND applicable
      </td>

      <td>
        String
      </td>

      <td>
        "dnd_timezone":"user",  
        OR  
        "dnd_timezone":"account"
      </td>
    </tr>

    <tr>
      <td>
        **when.campaign_cutoff***
      </td>

      <td>
        Stop sending messages from this campaign after a particular time (Campaign Cutoff)  
        Format : "HH:MM"
      </td>

      <td>
        Optional
      </td>

      <td>
        String (Time)
      </td>

      <td>
        "campaign_cutoff": "12:20"
      </td>
    </tr>
  </tbody>
</Table>

Example Payloads:

```json Web Push Notification
{
    "name": "My Webpush API campaign",
    "estimate_only": true,
 		"target_mode":"webpush",
		"teams": ["Team"],
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
// media & template_type is mandatory
```

```json Mobile Push Notification
{
    "name": "My API Campaign",
    "estimate_only": true,
    "target_mode":"push",
		"teams": ["Team"],
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
    "teams": ["Teams"],
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
// media & template_type is mandatory
```

```json SMS
{
    "name": "SMS Header,Message Key Values",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],
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
}
```

```json Webhooks
{
    "name": "My Webhook Campaign",
    "estimate_only": true,
    "target_mode":"webhook",
    "teams": ["Teams"],
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
    "teams": ["Teams"],
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
  	"custom_kv":[ //Supported for SDK version 5.2.0 or higher.
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
    "teams": ["Teams"], 
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
  "custom_kv":[  //Supported for SDK version 5.2.0 or higher.
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
```

```json App Inbox - Carousel Image
{    
    "name": "api simple test",
    "estimate_only": false,
    "target_mode":"notificationinbox",
    "teams": ["Teams"],

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
    "custom_kv":[  //Supported for SDK version 5.2.0 or higher.
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
```

```json App Inbox - Message with icon
{    
    "name": "api app inbox icon",
    "estimate_only": false,
    "target_mode":"notificationinbox",
    "teams": ["Teams"],
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
 		 "custom_kv":[
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
```

```json Whatsapp
{
    "name": "Standard Whatsapp campaign",
    "estimate_only": false,
    "target_mode": "whatsapp",
    "teams": ["Teams"],
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
    "teams": ["Team"],
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

> 📘 Note:
>
> Currently, Create Campaign APIs for WhatsApp do not support Carousel templates, Limited Time Offer (LTO) templates, Custom key-value pairs, and templates with copy code button.

```json Basic SMS Campaign Payload
{
    "name": "Sms Campaign API",
    "estimate_only": true,
    "target_mode": "sms",
    "teams": ["Team"],
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
}
```

```json SMS Campaign with Header Key-Value Pairs
{
    "name": "Sms Campaign API",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],  
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
}
```

```json SMS Campaign with Body Key-Value Pairs
{
    "name": "Sms Campaign API",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],
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
}
```

```json SMS Campaign with Template ID & Key-Value Pairs
{
    "name": "Target Key Value SMS",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],
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
}
```

# SMS

The Generic Sample Campaign API includes configurations for without Key Values, Key Value in Header Body, Key Value in Message Body, Target Level Key Value & Template ID, with payloads added below.

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
        Required/Optional
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
        $$header1
      </td>

      <td>
        The header key must match the key-value pairs defined in the Provider Setup headers.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        $$message1
      </td>

      <td>
        The message (body) key must match the key-value pairs defined in the Provider Setup body
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        1726822661
      </td>

      <td>
        This is the provider ID given by CleverTap, and you can obtain it from the URL when viewing the provider details in the dashboard or you can reach out to support team or account manager to get this value.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        name
      </td>

      <td>
        The name of your campaign shown in the CleverTap dashboard.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "My Campaign"
      </td>
    </tr>

    <tr>
      <td>
        provider_nickname
      </td>

      <td>
        Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is required for WhatsApp and SMS.
      </td>

      <td>
        Required
      </td>

      <td>
        String
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
        Allows you to filter target base on the user events and profile properties. Send an empty object ({'{}{}'}) to target your entire user base.
      </td>

      <td>
        Required
      </td>

      <td>
        Object
      </td>

      <td>
        "where": {'{'}  
        "event_name": "Charged",  
        "from": 20171001,  
        "to": 20171220,  
        "common_profile_properties": {'{'}  
        "profile_fields": [  
        {'{'}  
        "name": "Customer Type",  
        "operator":"equals",  
        "value": "Platinum"  
        {'}'}  
        ]  
        {'}'}\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        where.event_name
      </td>

      <td>
        Allows you to target users based on an event they perform.
      </td>

      <td>
        Optional
      </td>

      <td>
        String
      </td>

      <td>
        "charged"
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
        Optional
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
        Optional
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
        where.profile_fields
      </td>

      <td>
        Allows you to target users based on the values of their profile fields.
      </td>

      <td>
        Optional
      </td>

      <td>
        Array
      </td>

      <td>
        "common_profile_properties": {'{'}  
        "profile_fields": [  
        {'{'}  
        "name": "Customer Type",  
        "operator":"equals",  
        "value": "Platinum"  
        {'}'}  
        ]\

        {'}'}
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
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "now"
      </td>
    </tr>

    <tr>
      <td>
        content.body
      </td>

      <td>
        Body content for the SMS message.
      </td>

      <td>
        Required
      </td>

      <td>
        String
      </td>

      <td>
        "Hello User"
      </td>
    </tr>
  </tbody>
</Table>

```json Basic SMS Campaign Payload
{
    "name": "Sms Campaign API",
    "estimate_only": true,
    "target_mode": "sms",
    "teams": ["Team"],
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
}
```

```json SMS Campaign with Header Key-Value Pairs
{
    "name": "Sms Campaign API",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],  
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
}
```

```json SMS Campaign with Body Key-Value Pairs
{
    "name": "Sms Campaign API",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],
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
}
```

```json SMS Campaign with Template ID & Key-Value Pairs
{
    "name": "Target Key Value SMS",
    "estimate_only": false,
    "target_mode": "sms",
    "teams": ["Team"],
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

A successful response will return information about the created campaign, including a campaign ID and estimated reach.

## Example Request - Event Occurrence

You can target users based on specific event occurrences within a date range.

# Create Campaign API - Target Users by their Identities

You can send notifications to your users based on their Facebook ID, Email ID, custom-defined identity, or CleverTap ID.

You can message 1000 users per API request.

## Base URL - Target Users by their Identities

The endpoint is different based on the channel you are sending the message.

Following are some example base URLs from the account in the India region:

**SMS**  
[https://in1.api.clevertap.com/1/send/sms.json](https://in1.api.clevertap.com/1/send/sms.json)

**Push**  
[https://in1.api.clevertap.com/1/send/push.json](https://in1.api.clevertap.com/1/send/push.json)

> ❗️ Note:
>
> The push.json API currently does not support Pull Notifications.

**Web Push**  
[https://in1.api.clevertap.com/1/send/webpush.json](https://in1.api.clevertap.com/1/send/webpush.json)

**Email**  
[https://in1.api.clevertap.com/1/send/email.json](https://in1.api.clevertap.com/1/send/email.json)

> 📘 Region
>
> These are some example base URLs from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/create-campaign-api#region).

**WhatsApp**  
[https://in1.api.clevertap.com/1/send/whatsapp.json](https://in1.api.clevertap.com/1/send/whatsapp.json)

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
        "to": {'{'}  
        "FBID": ["102029292929388"],  
        "Email": ["[john@doe.com](mailto:john@doe.com)"],  
        "Identity": ["JohnDoe"],  
        "objectId": ["_asdnkansdjknaskdjnasjkndja"]\

        {'}'}
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
        optional
      </td>

      <td>
        array of strings
      </td>

      <td>
        "FBID": ["102029292929388","114342342453463"]
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
        optional
      </td>

      <td>
        array of strings
      </td>

      <td>
        "Email": ["[john@doe.com](mailto:john@doe.com)","[jane@doe.com](mailto:jane@doe.com)"]
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
        optional
      </td>

      <td>
        array of strings
      </td>

      <td>
        "Identity": ["JohnDoe"]
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
        optional
      </td>

      <td>
        array of strings
      </td>

      <td>
        "objectId": ["_asdnkansdjknaskdjnasjkndja"]
      </td>
    </tr>

    <tr>
      <td>
        tag_group
      </td>

      <td>
        Each message can be tagged into a tag group. Metrics, such as sent and viewed, will be shown against each tag group in the CleverTap dashboard. Max 500 groups. If not specified, the metrics will be shown against the "Not Tagged" tag. The stats page shows the tag group for the selected tag.
      </td>

      <td>
        optional (either use tag_group or campaign_id)
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
        "content": {'{'}  
        "subject": "Welcome",  
        "body": "<div>Your HTML content for the email</div>",  
        "sender_name": "CleverTap"\

        {'}'}
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
        required for push/web push
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
        "Have you seen the special offer?"
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
        required for email
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
        content.sender_name
      </td>

      <td>
        Sender name for your email which is sent to the user.
      </td>

      <td>
        required for email
      </td>

      <td>
        string
      </td>

      <td>
        "Onboarding"
      </td>
    </tr>

    <tr>
      <td>
        content.message_type
      </td>

      <td>
        Type of the  message. These can be two types:                 1. Template - Use when you are using a template to trigger the message               2. Freeform - When you are replying back to the user.
      </td>

      <td>
        Required (Applicable only for WhatsApp)
      </td>

      <td>
        String
      </td>

      <td>
        "template" or "freeform"
      </td>
    </tr>

    <tr>
      <td>
        content.template
      </td>

      <td>
        Contains all the information about the message content in the template.
      </td>

      <td>
        Required(Applicable only for WhatsApp)
      </td>

      <td>
        Object
      </td>

      <td>
        "template": {'{'}  
        "template_name": "Image_Template_1",  
        "header": {'{'}  
        "type": "image",  
        "media": {'{'}  
        "url": "[https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo\_1280x720\_5mb.jpg"\](https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo\\_1280x720\\_5mb.jpg"\\)
        {'}'}  
        {'}'},  
        "body": {'{'}  
        "type": "text",  
        "replacements": ["var1","Var2"]  
        {'}'},  
        "buttons": [  
        {'{'}  
        "replacements": [  
        "mvi qr payload 5"  
        ]  
        {'}'}  
        ]\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        content.template.template_name
      </td>

      <td>
        Name of the template used
      </td>

      <td>
        Required(Applicable only for WhatsApp)
      </td>

      <td>
        String
      </td>

      <td>
        "video_Template_1"
      </td>
    </tr>

    <tr>
      <td>
        content.template.language
      </td>

      <td>
        Language code for the language used to create the template. For more information, see [Meta Message Templates](https://developers.facebook.com/docs/whatsapp/api/messages/message-templates/#supported-languages)
      </td>

      <td>
        Required(Applicable only for WhatsApp)
      </td>

      <td>
        String
      </td>

      <td>
        "en"
      </td>
    </tr>

    <tr>
      <td>
        content.template.header
      </td>

      <td>
        Content information about the header used in the template. Used only if the template is using headers.
      </td>

      <td>
        Optional (Applicable only for WhatsApp)
      </td>

      <td>
        Object
      </td>

      <td>
        "header": {'{'}  
        "type": "image",  
        "media": {'{'}  
        "url": "[https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo\_1280x720\_5mb.jpg"\](https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo\\_1280x720\\_5mb.jpg"\\)
        {'}'}  
        {'}'}  
        For more header types, refer to the examples.
      </td>
    </tr>

    <tr>
      <td>
        content.template.header.type
      </td>

      <td>
        Type of the header (for example, video, image, location, text, or document).
      </td>

      <td>
        Required (Applicable only for WhatsApp)
      </td>

      <td>
        String
      </td>

      <td>
        "video"
      </td>
    </tr>

    <tr>
      <td>
        content.template.header.media
      </td>

      <td>
        Media-related information (URL of the video)
      </td>

      <td>
        Required (Applicable only for WhatsApp)
      </td>

      <td>
        Object
      </td>

      <td>
        "header": {'{'}  
        "type": "image",  
        "media": {'{'}  
        "url": "[https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo\_1280x720\_5mb.jpg"\](https://ia800701.us.archive.org/26/items/SampleVideo1280x7205mb/SampleVideo\\_1280x720\\_5mb.jpg"\\)
        {'}'}  
        {'}'}  
        For more header types, refer to the examples.
      </td>
    </tr>

    <tr>
      <td>
        content.template.header.replacements
      </td>

      <td>
        Replacement Object for text headers. Applicable only for text headers.
      </td>

      <td>
        Optional (Applicable only for WhatsApp)
      </td>

      <td>
        Object
      </td>

      <td>
        "header": {'{'}  
        "type": "text",  
        "replacements": [ "header var 1" ]\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        content.template.body
      </td>

      <td>
        Body section of the template
      </td>

      <td>
        Required (Applicable only for WhatsApp)
      </td>

      <td>
        Object
      </td>

      <td>
        "body": {'{'}  
        "type": "text",  
        "replacements": [  
        "Var 1", "Var 2"  
        ]\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        content.template.body.type
      </td>

      <td>
        Type of the body (text in this case)
      </td>

      <td>
        Required(Applicable only for WhatsApp)
      </td>

      <td>
        String
      </td>

      <td>
        "text"
      </td>
    </tr>

    <tr>
      <td>
        content.template.body.replacements
      </td>

      <td>
        Replacement variables in the body. A replacement value is required for each dynamic variable in the body.
      </td>

      <td>
        Required(Applicable only for WhatsApp)
      </td>

      <td>
        Array
      </td>

      <td>
        ["var1", "Var2"]
      </td>
    </tr>

    <tr>
      <td>
        content.template.buttons
      </td>

      <td>
        Button-related information
      </td>

      <td>
        Optional(Applicable only for WhatsApp)
      </td>

      <td>
        Array
      </td>

      <td>
        "buttons": [{'{'}  
        "replacements": [  
        "quick reply custom payload 1"  
        ]  
        {'}'},  
        {'{'}  
        "replacements": [  
        "quick reply custom payload 2"  
        ]  
        {'}'}  
        ]
      </td>
    </tr>

    <tr>
      <td>
        provider_nick_name
      </td>

      <td>
        Allows you to specify which vendor/provider to use for your campaign. For each target_mode, you can have multiple providers integrated with CleverTap, and this parameter allows you to select which provider to use for the campaign. For example, you select target_mode = email and you have integrated both SendGrid and Amazon SNS with CleverTap to send emails. In this scenario, you can set provider_nick_name = SendGrid to send the campaign through SendGrid. This parameter is required for WhatsApp and SMS.
      </td>

      <td>
        Required for WhatsApp and SMS
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
        respect_frequency_caps
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
        send_email_to_opted_out_users
      </td>

      <td>
        Valid values are: true and false. Set to false by default. When true, emails are sent to all qualified users, including those who have unsubscribed. For more information, refer to [Override Communication Preferences for Email](https://docs.clevertap.com/docs/create-message-email#override-communication-preferences-for-email).
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        "send_email_to_opted_out_users":true
      </td>
    </tr>

    <tr>
      <td>
        wzrk_cid
      </td>

      <td>
        Platform specific key that is required to target users on devices running Android O - can be used to determine notification channel to which a given push notification must belong.

        If a Notification ID is mandatory then the channel ID is expected. An error occurs if a valid channel ID is not received.
      </td>

      <td>
        mandatory for Android O
      </td>

      <td>
        string
      </td>

      <td>
        "Marketing"
      </td>
    </tr>

    <tr>
      <td>
        badge_id
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
        1
      </td>
    </tr>

    <tr>
      <td>
        badge_icon
      </td>

      <td>
        Platform specific key that can be used while targeting users on devices running Android O - can be used to determine which icon should be used.

        Accepted values:  
        app icon  
        notification icon
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "app icon"
      </td>
    </tr>

    <tr>
      <td>
        mutable-content
      </td>

      <td>
        To raise Notification viewed event for Push notifications, raise the flag to "true" in the platform_specific: ios section of the request payload.
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
        platform_specific
      </td>

      <td>
        The platform-specific details for Push and Web Push notification
      </td>

      <td>
        optional
      </td>

      <td>
        object
      </td>

      <td>
        {'{'}  
        "ios": {'{'}  
        "deep_link": "example.com"  
        {'}'}\

        {'}'}
      </td>
    </tr>

    <tr>
      <td>
        background_image
      </td>

      <td>
        The parameter to render images in the push notification for Android.
      </td>

      <td>
        Optional
      </td>

      <td>
        string
      </td>

      <td>
        "[http://example.jpg](http://example.jpg)"
      </td>
    </tr>

    <tr>
      <td>
        ct_mediaUrl
      </td>

      <td>
        The parameter to render images in the push notification for iOS.
      </td>

      <td>
        Optional
      </td>

      <td>
        string
      </td>

      <td>
        "[https://example.jpg](https://example.jpg)"
      </td>
    </tr>

    <tr>
      <td>
        wzrk_clr
      </td>

      <td>
        A Platform specific key that can be used to set color for small icons in push notifications.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "#1DA1F2"
      </td>
    </tr>

    <tr>
      <td>
        respect_communication_preference
      </td>

      <td>
        Send this value as _False_ to override the communication preferences of the user. For example, transaction updates, user replies, and so on. Check that you do not breach any compliance regulations by overriding the user communication preferences.
      </td>

      <td>
        Optional (Applicable only for WhatsApp)
      </td>

      <td>
        Boolean
      </td>

      <td>
        false
      </td>
    </tr>

    <tr>
      <td>
        message_id
      </td>

      <td>
        A unique identifier to track the status of each message sent to the end user. The limit is 8 characters.
      </td>

      <td>
        Required(Applicable only for WhatsApp)
      </td>

      <td>
        String
      </td>

      <td>
        "4536tsyt"
      </td>
    </tr>
  </tbody>
</Table>

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
                    "url": "https://d2h5ndkimzo5f0.cloudfront.net/dist/1655455488/i/d024319ee2bf46cdbafbbe0d02afdb00.mp3?v=1686580980"
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

A successful response implies payload validity and addition to the processing queue. Message delivery is handled by the provider (push or email) and depends on if you provided a valid recipient id.

## Errors

If there are errors, you will receive a response in the format below.

Following are the possible error codes:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Error Code
      </th>

      <th>
        Error Message
      </th>

      <th>
        Troubleshooting Steps
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        21
      </td>

      <td>
        "to" is a mandatory field"
      </td>

      <td>
        Check if the _to_ field in the payload is missing recipient records.
      </td>
    </tr>

    <tr>
      <td>
        73
      </td>

      <td>
        respect_frequency_caps
      </td>

      <td>
        The value must be Boolean.
      </td>
    </tr>

    <tr>
      <td>
        76
      </td>

      <td>
        "tag_group" has to be string
      </td>

      <td>
        Check if the value for tag group is _string_.
      </td>
    </tr>

    <tr>
      <td>
        76
      </td>

      <td>
        "message_type" has to be string
      </td>

      <td>
        Check if the value for the Message Type is string.
      </td>
    </tr>

    <tr>
      <td>
        78
      </td>

      <td>
        Invalid payload pls update and try again
      </td>

      <td>
        Invalid JSON payload. Verify the payload.
      </td>
    </tr>

    <tr>
      <td>
        84
      </td>

      <td>
        "Invalid recipients"
      </td>

      <td>
        Recipient information mentioned in the payload does not exist in the CleverTap dashboard.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        Unexpected error, please try again
      </td>

      <td>
        The message failed to send due to an unknown error.

        Try again. If the error persists, open a support ticket from the dashboard.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        The WhatsApp provider you've selected is not supported with your current WhatsApp add-on. To use this provider, please enable the WhatsApp Connect add-on
      </td>

      <td>
        1. Check that you are not using external providers in the Send Message API .  
           2. Check your organization settings to determine if the  WhatsApp Connect Add-on is enabled for your project. If the error persists even if the add-on is enabled, please contact our support.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        The WhatsApp provider you've selected is not supported with your current WhatsApp add-on. To use this provider, please enable the WhatsApp Direct add-on
      </td>

      <td>
        1. Check that you are not using CleverTap BSP in the Send Message API .  
           2. Check your organization settings to determine if the  WhatsApp Direct Add-on is enabled for your project. If the error persists even if the add-on is enabled, please contact our support.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        "provider_nick_name" can't be empty
      </td>

      <td>
        The provider nick name is mandatory for WhatsApp campaigns. Check if the provider_nick_name is missing or empty.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        The provider_nick_name used in request does not exist in your account.
      </td>

      <td>
        Check if the provider used in the message payload exists in the CleverTap dashboard. To check, navigate to Settings {'>'} Channel {'>'} WhatsApp.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        "message_id" length can't be greater than 8
      </td>

      <td>
        Check if the length of the message ID is greater than 8 characters.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        "message_id" can't be empty
      </td>

      <td>
        Check if the value for the Message ID is missing.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        Received invalid message type
      </td>

      <td>
        The only accepted values are "template" or "freeform".
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        Template not found for the specified provider
      </td>

      <td>
        Check if the template used in the payload exists for the specified provider. Navigate to Settings {'>'} Channel {'>'} WhatsApp{'>'} [provider name] to check the template. To fix this, either use a provider that has the specified template or use the template that is available for the provider mentioned in the payload.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        Template name is mandatory
      </td>

      <td>
        Check if the template name is missing or empty.
      </td>
    </tr>

    <tr>
      <td>
        89
      </td>

      <td>
        Invalid number of body/header/button replacements specified, the body/header/button has [n] replacement(s).
      </td>

      <td>
        The number of variables and values in the payload must match for body, header, or button.
      </td>
    </tr>

    <tr>
      <td>
        * <br />
      </td>

      <td>
        Team name is missing. Add a team and try again.
      </td>

      <td>
        Ensure the teams parameter is present and not empty.
      </td>
    </tr>

    <tr>
      <td>
        * <br />
      </td>

      <td>
        Invalid syntax in Team field. Rectify and try again.
      </td>

      <td>
        The teams field must be a valid list of strings.
      </td>
    </tr>

    <tr>
      <td>
        * <br />
      </td>

      <td>
        A campaign can be created for only one team. Keep only one team and try again.
      </td>

      <td>
        Check if there is only one team present in the list.
      </td>
    </tr>

    <tr>
      <td>
        * <br />
      </td>

      <td>
        Team with entered name doesn't exist.
      </td>

      <td>
        Check if the specified team exists in your CleverTap account.
      </td>
    </tr>

    <tr>
      <td>
        * <br />
      </td>

      <td>
        Selected segment can't be used because it belongs to another team. Change segment and try again.
      </td>

      <td>
        Use a segment that belongs to the same team as the campaign.
      </td>
    </tr>
  </tbody>
</Table>

## View Campaigns Created Through APIs

To view campaigns created through APIs:

1. Navigate to the _Campaigns_ section from the CleverTap dashboard. The list of campaign displays.
2. Click the Filter icon {'>'} _Created by_ filter and select the _serverapi{'<channel name>'}_ option.

<Image align="center" alt="Filter API Campaigns" border={true} caption="Filter API Campaigns" src="https://files.readme.io/ee5427a-small-Filter_campaigns_by_API.png" />

3. Click **Apply**. All the related campaigns are displayed.
4. Click any of the campaigns to view details and stats.

# Unsubscribe Link for Emails

If you're using Amazon SES or your own SMTP gateway, you'll have to follow the steps [listed on this page](https://docs.clevertap.com/docs/handling-unsubscribes) to enable your users to unsubscribe from your email notifications.

# FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

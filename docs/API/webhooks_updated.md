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

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Type
      </th>

      <th>
        Description
      </th>

      <th>
        Example
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Single action
      </td>

      <td>
        User performed an action.
      </td>

      <td>
        User launched the app.
      </td>
    </tr>

    <tr>
      <td>
        Actions
      </td>

      <td>
        User performed multiple actions.
      </td>

      <td>
        User launched the app and viewed a notification.
      </td>
    </tr>

    <tr>
      <td>
        Inaction within time
      </td>

      <td>
        User performed an action, but does not perform another action within a certain time.
      </td>

      <td>
        User has not launched the app in three days.
      </td>
    </tr>

    <tr>
      <td>
        Inaction
      </td>

      <td>
        Users performed an action, but did not perform another action.
      </td>

      <td>
        User launched the app, but did not view a notification.
      </td>
    </tr>

    <tr>
      <td>
        Date time property
      </td>

      <td>
        Period of time after a date time property on a user event.
      </td>

      <td>
        User purchased an item and three days have passed.
      </td>
    </tr>

    <tr>
      <td>
        Actions with user properties
      </td>

      <td>
        Performed an action or not, filtered by their demographic properties.
      </td>

      <td>
        User purchased an item and lives in Canada.
      </td>
    </tr>
  </tbody>
</Table>

Webhooks are created in the CleverTap dashboard by defining a user segment and a callback URL on your server. CleverTap will send a webhook as a HTTP POST with information about the event when it occurs. You can customize the webhooks to use basic authentication and include any custom properties if needed. 

The rest of this page walks through how to create your first CleverTap webhook. 

# Step 1: Set Up Webhook in CleverTap Dashboard

Go to Settings>  Engage > Channels > Webhooks.\
Click the +Add Webhook button.\
Update the webhook template.

<Image title="Campaign_Webhooks_add_webhook_param.png" alt={606} className="border" width="100%" border={true} src="https://files.readme.io/c5ed2e7-Campaign_Webhooks_add_webhook_param.png" />

> 📘 Note
>
> Add a Webhook name and Destination URL. If you currently don’t have an endpoint created on your server to receive the webhook, you can use [www.Requestbin.com](http://www.Requestbin.com) to create an endpoint for testing purposes.

Click Save.

# Step2: Create a Webhook Campaign

Go to the Campaigns from the CleverTap dashboard. Click the + Campaign button. The channels page appears. Click "Webhooks" under Server channels. 

<Image title="Campaign_Webhooks_channel_select.png" alt={850} width="100%" border={true} src="https://files.readme.io/ba80bca-Campaign_Webhooks_channel_select.png">
  Image replacement
</Image>

Select the campaign type. 

<Image title="Campaign_Webhooks_campaign_type.png" alt={1066} className="border" border={true} src="https://files.readme.io/1a570eb-Campaign_Webhooks_campaign_type.png" />

Select when to send the campaign. 

<Image title="Campaign_Webhooks_campaign_When.png" alt={1106} width="100%" src="https://files.readme.io/14998f1-Campaign_Webhooks_campaign_When.png" />

If you have an existing segment that you want use for the webook, select that segment. Otherwise, click the + Create ad-hoc segment button. 

<Image title="Campaign_Webhooks_campaign_Segments.png" alt={1066} border={true} src="https://files.readme.io/9e9200a-Campaign_Webhooks_campaign_Segments.png">
  Image replacement
</Image>

In the Single action box, click + Create an ad-hoc segment. Enter details and click Continue.

<Image title="Campaign_Webhooks_campaign_What.png" alt={1110} border={true} src="https://files.readme.io/0172d69-Campaign_Webhooks_campaign_What.png">
  Image replacement
</Image>

* Select Webhook URL from the list. 
* Add query parameters in URL key-value pairs. 

<Image title="Campaign_Webhooks_Message_compose_main.png" alt={673} width="100%" src="https://files.readme.io/bec6e34-Campaign_Webhooks_Message_compose_main.png" />

> 📘 Note
>
> You can also click the "Add a new webhook URL" to create a new Webhook URL. You will be redirected to the settings page. See [Set up Webhook](https://developer.clevertap.com/docs/webhooks_updated#section-step-1-set-up-webhook-in-clever-tap-dashboard).

* Select the content-type (JSON or plain text)

* Add the Webhook content to send or receive the data. You can either use Profile Variables or Custom body.  You can also add personalization with "@" here. You have the option to send/receive email, identity, objectId (CleverTap ID), profileData (custom profile variables), and push\_token for each user.

<Image title="Campaign_Webhooks_Message_compose_profile_variables_zoom.png" alt={711} className="border" width="100%" border={true} src="https://files.readme.io/2fc7d82-Campaign_Webhooks_Message_compose_profile_variables_zoom.png" />

You can also choose a custom payload. 

<Image title="Campaign_Webhooks_Message_compose_custom_payload_zoom.png" alt={685} className="border" width="100%" border={true} src="https://files.readme.io/7cfff5b-Campaign_Webhooks_Message_compose_custom_payload_zoom.png" />

The preview for your payload is displayed on the right side of the pane. The final view will look something similar:

<Image title="Campaign_Webhooks_Preview.png" alt={1353} className="border" width="100%" border={true} src="https://files.readme.io/72e99b7-Campaign_Webhooks_Preview.png" />

Click the "send a test webhook" link to test if your Webhook is set up and working correctly. 

Apply control groups, if required. 

![1380](https://files.readme.io/25b0f14-Campaign_Webhooks_campaign_setup_control_groups.png "Campaign_Webhooks_campaign_setup_control_groups.png")

Click Continue. Check the Overview and deploy the Webhook. 

<Image title="Campaign_Webhooks_campaign_Overview.png" alt={1069} className="border" width="100%" border={true} src="https://files.readme.io/07443ab-Campaign_Webhooks_campaign_Overview.png" />

# Step 3: Listen for Webhooks on Your Server

In the previous step, we created a webhook to send notifications from our server when a user makes a purchase. 

<Image title="image11.png" alt={738} border={true} src="https://files.readme.io/c361c62-image11.png">
  Image replacement
</Image>

> 🚧 Understanding Webhook errors
>
> Webhook is a request-response type format. CleverTap sends the request and waits for one second for a response from the end point. If the end point URL fails to respond within one second, CleverTap retries the request. If the retry also fails, the campaign stats page is updated with the error 'Webhook Dispatch Failed'. 
>
> Also, another probable cause for the error is if the webhook endpoint is temporarily unavailable or down.

Below is a sample webhook payload containing all possible fields for profile variables. The TargetId corresponds to the webhook campaign ID for which you are receiving the payload. 

```json
{
    "targetId": 1472124953,
    "key_values": {
        "PromoCode": "MT50",
        "PowerUser": "true"
    },
    "profiles": [
        {
            "Email": "jack@gmail.com",
            "Identity": "foo",
            "ObjectId": "-g55b74fb1030740e4a4931910a8abb862",
            "ProfileData": {
                "Last Score": 308,
                "High Score": 308,
                "Replayed": true
            },
            "Event_properties": {
                "Score": 1200,
                "Game Mode": "Practice",
                "Rating": 1
            },
            "Push_token": "19403aa5d3bb376769a8101c9cbf22159cb1836117659cc684a71243589982ea"
        },
        {
            "Email": "jill@gmail.com",
            "Identity": "bar",
            "ObjectId": "__g09c9bf3b0d374a259c86f5b855ec9b19",
            "ProfileData": {
                "Last Score": 309,
                "High Score": 309,
                "Replayed": true
            },
            "Event_properties": {
                "Score": 500,
                "Game Mode": "Tournament",
                "Rating": 2
            },
            "Push_token": "fiCjRO_opjw:APA91bH0yzS_eBj1Xx8TcakMzz4yTc3id29A79GOjnaaMtIQLrjztNfzzRz7slqWmNzndECx_hHh7qiL0LZJOkGwzr9Zp_g1mC9B6BVuEbsjrMFoxXX830zMRuvxtU_AhFTC0JPHejZh"
        }
    ]
}
```

# View Statistics

You can view the statistics of your Webhook from the Campaigns list page.

<Image title="Campaign_Webhooks_Statistics.png" alt={1381} className="border" width="100%" border={true} src="https://files.readme.io/8a47af5-Campaign_Webhooks_Statistics.png" />

Congratulations on creating your first CleverTap webhook!

Another option besides setting up an endpoint on your server to listen for CleverTap webhooks is using a workflow automation tool, such as Tray.io or Zapier. These tools can help integrate a CleverTap webhook without any code and then send these notifications to other systems such as Slack. If you are interested in learning more, we have [a guide](https://docs.clevertap.com/docs/send-abandoned-cart-users-to-slack) that walks through sending a CleverTap webhook to Slack via Tray.io.

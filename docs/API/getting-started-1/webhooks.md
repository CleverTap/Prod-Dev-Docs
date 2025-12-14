---
title: Webhooks
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
# Overview

CleverTap webhooks enable developers to monitor specific user events and receive push notifications to their server when those events happen. CleverTap webhooks are often used to trigger business workflows. For example, if you want to get notified every time a user looks up an international flight in business class but then doesn’t book the flight within 30 minutes. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a process in your call center to contact the user to complete the booking.

Another popular use case for CleverTap webhooks is with e-commerce companies and cart abandonment. For example, if a user abandons their cart during a high-value transaction. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a call center workflow to contact the user to solve any issues and complete the purchase. 

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

Webhooks are created in the CleverTap dashboard by defining a user segment and a callback URL on your server. CleverTap sends a webhook as an HTTP POST with information about the event when it occurs. You can customize the webhooks to use basic authentication and include any custom properties if needed. 

# Set Up Webhook in CleverTap Dashboard

To set up your first CleverTap webhook, perform the following steps:

1. Navigate to *Settings* > *Engage* > *Channels* > *Webhooks*.
2. Click **+ Add Webhook**. 
3. Update the webhook template.
4. Click **Save**.

<Image title="Campaign_Webhooks_add_webhook_param.png" alt={606} className="border" width="smart" border={true} src="https://files.readme.io/c5ed2e7-Campaign_Webhooks_add_webhook_param.png" />

> 📘 Testing Considerations
>
> Add a webhook name and destination URL. If you currently do not have an endpoint created on your server to receive the webhook, you can use [www.Requestbin.com](http://www.Requestbin.com) to create an endpoint for testing purposes.

# Create a Webhook Campaign

To create a webhook campaign, perform the following steps:

1. Navigate to *Campaigns* from the CleverTap dashboard. 
2. Click **+ Campaign**. The channels page displays. 
3. Click **Webhooks** under *Server* channels. 

<Image title="Campaign_Webhooks_channel_select.png" alt={850} className="border" width="100%" border={true} src="https://files.readme.io/ba80bca-Campaign_Webhooks_channel_select.png" />

4. Select the campaign type. 

<Image title="Campaign_Webhooks_campaign_type.png" alt={1066} className="border" border={true} src="https://files.readme.io/1a570eb-Campaign_Webhooks_campaign_type.png" />

5. Select when to send the campaign. 

<Image title="Campaign_Webhooks_campaign_When.png" alt={1106} width="100%" src="https://files.readme.io/14998f1-Campaign_Webhooks_campaign_When.png" />

> 📘 Recurring Day
>
> If you specify a recurring day for a campaign, such as the 7th of each month, then the campaign will start for the specified day and ignore the creation date. This is a precaution to avoid sending a campaign unintentionally on a prior date.

6. If you have an existing segment you want to use for the webhook, select that segment. Otherwise, click **+ Create an ad-hoc segment**. 

<Image title="Campaign_Webhooks_campaign_Segments.png" alt={1066} className="border" border={true} src="https://files.readme.io/9e9200a-Campaign_Webhooks_campaign_Segments.png" />

7. In the *Single action box*, click **+ Create an ad-hoc segment**, and then enter details and click **Continue**.

<Image title="Campaign_Webhooks_campaign_What.png" alt={1110} className="border" border={true} src="https://files.readme.io/0172d69-Campaign_Webhooks_campaign_What.png" />

8. Select *webhook URL* from the list. 
9. Add query parameters in the URL key-value pairs section. 

<Image title="Campaign_Webhooks_Message_compose_main.png" alt={673} width="100%" src="https://files.readme.io/bec6e34-Campaign_Webhooks_Message_compose_main.png" />

> 📘 Note
>
> You can also click the **Add a new webhook URL** to create a new Webhook URL. You will be redirected to the settings page. Refer to [Set up a Webhook](https://developer.clevertap.com/docs/webhooks#section-step-1-set-up-webhook-in-clever-tap-dashboard).

10. Select the content type (JSON or plain text).
11. Add the webhook content to send or receive the data. 

You can either use *Profile Variables* or *Custom* body.  You can also add personalization with the @ symbol. You have the option to send/receive an email, identity, objectId (CleverTap ID), profileData (custom profile variables), and push\_token for each user.

<Image title="Campaign_Webhooks_Message_compose_profile_variables_zoom.png" alt={711} className="border" width="100%" border={true} src="https://files.readme.io/2fc7d82-Campaign_Webhooks_Message_compose_profile_variables_zoom.png" />

You can also choose a custom payload. 

<Image title="Campaign_Webhooks_Message_compose_custom_payload_zoom.png" alt={685} className="border" width="100%" border={true} src="https://files.readme.io/7cfff5b-Campaign_Webhooks_Message_compose_custom_payload_zoom.png" />

The preview for your payload is displayed on the right side of the pane. The final view will look similar to the following:

<Image title="Campaign_Webhooks_Preview.png" alt={1353} className="border" width="100%" border={true} src="https://files.readme.io/72e99b7-Campaign_Webhooks_Preview.png" />

### Liquid Tags

Liquid tags offer flexibility in composing your message. You can have fixed or variable values and change the look and feel of your message by using Liquid tags. 

![1357](https://files.readme.io/ed642bf-campaigns_liquid_tags_common_example.png "campaigns_liquid_tags_common_example.png")

Each notification is personalized to the receiver. 

```html Output - Liquid
Hello John!

Here is a special coupon for you: Gold4All
```

For more information on using tags, refer to [Liquid Tags](https://docs.clevertap.com/docs/liquid-tags#using-tags).

### Linked Content

Linked content gives you the ability to personalize messages with rich and contextual data that is available outside of the CleverTap platform. Linked Content helps you to send messages with send-time personalization.

For example, you deliver food across different geographies and you want to personalize offers to each user based on the weather on their location. The location can come from CleverTap personalization and the weather information can come from an API that provides the current weather. If you want to add more information, you can opt for any source such as third-party tools and in-house software to include in your message.

For more information on using Linked Content, refer to [Linked Content](https://docs.clevertap.com/docs/linked-content#write-linked-content).

To use linked content, perform the following steps:

1. Click the **send a test webhook** link to test if your webhook is set up and working correctly. 
2. Apply control groups, if required. 

![1380](https://files.readme.io/25b0f14-Campaign_Webhooks_campaign_setup_control_groups.png "Campaign_Webhooks_campaign_setup_control_groups.png")

3. Click **Continue**. Check the *Overview* and deploy the webhook. 

<Image title="Campaign_Webhooks_campaign_Overview.png" alt={1069} className="border" width="100%" border={true} src="https://files.readme.io/07443ab-Campaign_Webhooks_campaign_Overview.png" />

# Listen for Webhooks on Your Server

In the previous step, we created a webhook to send notifications from our server when a user makes a purchase. 

<Image title="image11.png" alt={738} className="border" border={true} src="https://files.readme.io/c361c62-image11.png" />

> 🚧 Understanding Webhook Errors
>
> Webhook is a request-response type format. CleverTap sends the request and waits for three seconds for a response from the endpoint. If the endpoint URL fails to respond then CleverTap retries the request. If the retry also fails, the campaign stats page is updated with the error "Webhook Dispatch Failed." 
>
> Also, another probable cause for the error is if the webhook endpoint is temporarily unavailable or down.

## Sample Payload

Following is a sample webhook payload containing all possible fields for profile variables. The targetId corresponds to the webhook campaign ID for which you are receiving the payload. 

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

You can view the statistics of your webhook from the *Campaign* list page.

<Image title="Campaign_Webhooks_Statistics.png" alt={1381} className="border" width="100%" border={true} src="https://files.readme.io/8a47af5-Campaign_Webhooks_Statistics.png" />

Another option besides setting up an endpoint on your server to listen for CleverTap webhooks is using a workflow automation tool, such as Tray.io or Zapier. These tools can help integrate a CleverTap webhook without any code and then send these notifications to other systems such as Slack. If you are interested in learning more, we have [a guide](https://docs.clevertap.com/docs/send-abandoned-cart-users-to-slack) that walks through sending a CleverTap webhook to Slack via Tray.io.

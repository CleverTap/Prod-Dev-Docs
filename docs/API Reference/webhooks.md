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

Please note this is feature is only available to paid accounts. To enable webhooks, please upgrade your account to a paid plan.

The rest of this page walks through how to create your first CleverTap webhook. 

# Step 1: Set Up Webhook in CleverTap Dashboard

Go to the Messages page of the CleverTap dashboard. Click on the + Message button.

<Image title="image1.png" alt={1039} className="border" border={true} src="https://files.readme.io/eb2f165-image1.png" />

Click on the Webhooks button.

<Image title="image12.png" alt={272} className="border" border={true} src="https://files.readme.io/e1d666c-image12.png" />

If you have an existing segment that you want use for the webook, select that segment. Otherwise, click the + Create ad-hoc segment button. 

<Image title="image10.png" alt={1045} className="border" border={true} src="https://files.readme.io/ba7ee12-image10.png" />

In the Single action box, click + Create segment

<Image title="image2.png" alt={1027} className="border" border={true} src="https://files.readme.io/c5219bb-image2.png" />

Choose a name for the webhook. Click Continue to When.

<Image title="image4.png" alt={791} className="border" border={true} src="https://files.readme.io/b20db4b-image4.png" />

On the When page, you keep the default settings and click Continue to Who. If you want to choose a different start date, end date, or add a delay for the webhook, you can modify it on this page. 

<Image title="image7.png" alt={733} className="border" border={true} src="https://files.readme.io/f49c0f9-image7.png" />

Under As Soon as User Does, select App Launched. Click Continue to What.

<Image title="image8.png" alt={572} className="border" border={true} src="https://files.readme.io/43b8692-image8.png" />

Click Add a new webhook URL, which is where CleverTap will send the webhook. 

<Image title="image5.png" alt={778} className="border" border={true} src="https://files.readme.io/82ba26e-image5.png" />

Click + Add new

<Image title="image6.png" alt={850} className="border" border={true} src="https://files.readme.io/eda3456-image6.png" />

Add a Webhook name and Destination URL. If you currently don’t have an endpoint created on your server to receive the webhook, you can use Requestb.in to create an endpoint for testing purposes. Click Save.

![428](https://files.readme.io/d14df94-image9.png "image9.png")

Under Webhook URL, select the one you just created. You have the option to receive email, identity, objectId (CleverTap ID), profileData (custom profile variables), and push\_token for each user.

![749](https://files.readme.io/79a754b-image13.png "image13.png")

Click Continue to Overview and then Click Deploy Webhook.

<Image title="image3.png" alt={191} className="border" border={true} src="https://files.readme.io/1fef99d-image3.png" />

# Step 2: Listen for Webhooks on Your Server

In the previous step, we created a webhook to send notifications to our server when a user launches our app. 

If you have CleverTap already integrated in your app, try launching your app now. Then go back to the Messages page in the dashboard. You will see a webhook was sent to your server. 

<Image title="image11.png" alt={738} className="border" border={true} src="https://files.readme.io/c361c62-image11.png" />

Below is a sample webhook payload containing all possible fields. targetId corresponds to the webhook campaign ID for which you are receiving the payload.

```json
{
    "targetId": 1472124953,
    "key_values": {
        "PromoCode": "MT50",
        "PowerUser": "true"
    },
    "profiles": [
        {
            "email": "jack@gmail.com",
            "identity": "foo",
            "objectId": "-g55b74fb1030740e4a4931910a8abb862",
            "profileData": {
                "Last Score": 308,
                "High Score": 308,
                "Replayed": true
            },
            "event_properties": {
                "Score": 1200,
                "Game Mode": "Practice",
                "Rating": 1
            },
            "push_token": "19403aa5d3bb376769a8101c9cbf22159cb1836117659cc684a71243589982ea"
        },
        {
            "email": "jill@gmail.com",
            "identity": "bar",
            "objectId": "__g09c9bf3b0d374a259c86f5b855ec9b19",
            "profileData": {
                "Last Score": 309,
                "High Score": 309,
                "Replayed": true
            },
            "event_properties": {
                "Score": 500,
                "Game Mode": "Tournament",
                "Rating": 2
            },
            "push_token": "fiCjRO_opjw:APA91bH0yzS_eBj1Xx8TcakMzz4yTc3id29A79GOjnaaMtIQLrjztNfzzRz7slqWmNzndECx_hHh7qiL0LZJOkGwzr9Zp_g1mC9B6BVuEbsjrMFoxXX830zMRuvxtU_AhFTC0JPHejZh"
        }
    ]
}
```

# Next Steps

Congrats on creating your first CleverTap webhook!

Another option besides setting up an endpoint on your server to listen for CleverTap webhooks is using a workflow automation tool, such as Tray.io or Zapier. These tools can help integrate a CleverTap webhook without any code and then send these notifications to other systems like Slack. If you are interested in learning more, we have [a guide](https://community.clevertap.com/t/send-your-cart-abandoned-users-to-your-slack-channel/649) that walks through sending a CleverTap webhook to Slack via Tray.io.

As a next step, you can explore the [Get Events API](doc:get-events-api), which will let you download user events from CleverTap.

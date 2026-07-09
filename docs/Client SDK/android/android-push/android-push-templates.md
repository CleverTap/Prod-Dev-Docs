---
title: Android Push Templates
excerpt: >-
  Learn how to implement and customize Push Notification for your Android
  device.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The CleverTap Push Templates SDK enhances user engagement through rich, customizable push notification templates that work seamlessly with CleverTap Android SDK  v7.4.1 and above.

# Prerequisites

To use the CleverTap Push Templates SDK, you must integrate the CleverTap Android SDK version v7.4.1 or above.

# Installation

The Android Push Templates SDK supports the following two integration options:

- [Out-of-the-box Implementation](doc:android-push-templates#configure-out-of-the-box-push-notification-handling) : Uses CleverTap's default notification handling, where templates are rendered without additional customization or code changes in your app.
- [Custom Implementation](doc:android-push-templates#customize-push-notification-handling): Allows you to control how push notifications are received and displayed.

## Configure Out-of-the-Box Push Notification Handling

To configure out-of-the-box Push Notification Handling, perform the following steps:

1. Add the dependencies to the `build` as given in the following code:

```groovy
implementation "com.clevertap.android:push-templates:2.2.0" // 2.1.0 and above
implementation "com.clevertap.android:clevertap-android-sdk:7.8.0" // 7.4.1 and above
```

2. Add the following line of code to your Application class:

```java
CleverTapAPI.setNotificationHandler((NotificationHandler)new PushTemplateNotificationHandler());
```
```kotlin
CleverTapAPI.setNotificationHandler(PushTemplateNotificationHandler() as NotificationHandler);
```

## Customize Push Notification Handling

To customize the Push Notification handling, add the following code in your custom `FirebaseMessageService` class:

```java
public class PushTemplateMessagingService extends FirebaseMessagingService {
    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        CTFcmMessageHandler()
                .createNotification(getApplicationContext(), remoteMessage);
    }
}
```
```kotlin
class MyFcmMessageListenerService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)
            CTFcmMessageHandler()
                .createNotification(applicationContext, message)
    }
}
```

# Supported Push Templates

You can implement the following customizable push notification templates using the CleverTap Android Push Template Library SDK. The templates can be broadly classified as follows:

**Basic Templates**

- [Standard Push Template](doc:android-push-templates#standard-push-template)
- [Advanced Push Template](doc:android-push-templates#advanced-push-template)

**Carousel Templates**

- [Auto Carousel Template](doc:android-push-templates#auto-carousel-template)
- [Manual Carousel Template](doc:android-push-templates#manual-carousel-template)

**Advanced Templates**

- [Timer Template](doc:android-push-templates#timer-template)
- [Text over Image Template](doc:android-push-templates#text-over-image-template)
- [Five Icons Template](doc:android-push-templates#five-icons-template)
- [Rating Template](doc:android-push-templates#rating-template)
- [Product Catalog Template](doc:android-push-templates#product-catalog-template)
- [Input Box Template](doc:android-push-templates#input-box-template)
- [Vertical Image CTA](https://developer.clevertap.com/docs/android-push-templates#vertical-image-with-cta-template)

# Standard Push Template

Standard Template is a default template for push notifications received on apps that supports a simple layout with title, message, image, and optional buttons, ideal for quick, informative alerts. For example, an e-commerce business might want to inform customers that their order is on the way. To use a GIF in a standard push template, pass the key `wzrk_gif` with the value set to the URL of the GIF image.


<Image src="https://files.readme.io/7a4e66924248b609d31ccdc200b05788c5a2b1b5e6550e58de559233caefe4b2-image.png" alt="Standard Push Template" align="center" width="35% " caption="Standard Push Template" />


# Advanced Push Template

This template supports rich media, deep links, and enhanced customization for more engaging messages. It is ideal for interactive, visually appealing messages. For example, businesses can use it to build excitement for new product drops, early access offers, or feature rollouts that benefit from strong visuals and clear Call to Action (CTAs).

For template outputs Android 11 and below, refer to [Push Templates on Android v11 and below](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESANDROID12.md).


<Image src="https://files.readme.io/65cc6bcd2523518ceab5cdc452022fbc4f4bce889e87469c883fd394b1fb18e4-image.png" alt="Advanced Push Template" align="center" width="35% " caption="Advanced Push Template" />


## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "right" }}>
        Advanced Template Keys
      </th>

      <th style={{ textAlign: "center" }}>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_id
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_basic`.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_summary
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Message line when Notification is expanded.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_subtitle
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background color of your push notification. You can use it to match your brand theme or draw visual attention to the notification. It accepts HEX codes or color pickers.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Background color of your push notification in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif\_frames
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_ico
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        wzrk\_hide\_large\_icon
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications<br />Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl1
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        One Deep Link (minimum)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_scale\_type
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li><li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_sticky
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dismiss
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_json
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Above template keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Auto Carousel Template

Auto Carousel is an automatic revolving carousel push notification that automatically cycles through multiple cards or slides within a single notification. Each card typically includes an image, title, description, and a CTA button, allowing users to view multiple offers, messages, or product highlights without leaving their device’s notification tray.


<Image src="https://files.readme.io/b06f01f282ddb61ab6bca44b51c79e65c9735b224432fadaf0799eb6c1dd7a70-Screen_Recording_Jun_4_2025_from_Google_Drive.mp4_1.gif" alt="Auto Carousel Template" align="center" width="35% " caption="Auto Carousel Template" />


## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "right" }}>
        Auto Carousel Template Keys
      </th>

      <th style={{ textAlign: "center" }}>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_id
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_carousel`.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_summary
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message line when Notification is expanded.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_subtitle
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl1
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Deep Link (Max one)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img1
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image One
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img1\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image One
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img2
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image Two
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img2\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image Two
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img3
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image Three
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img3\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image Three
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img`n`
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Image `N`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt _img_`n`alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image `N`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_ico
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        wzrk\_hide\_large\_icon
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications<br />Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_sticky
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dismiss
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_scale\_type
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li><li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_json
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Manual Carousel Template

Manual Carousel is an interactive push notification format that allows users to browse through multiple cards or slides at their own pace. Each card typically features rich content such as images, titles, short descriptions, and a CTA. Navigation is user-controlled, with arrows or swipe gestures enabling the transition between cards.

If only a single image is available for download, this template falls back to the Basic Template.


<Image src="https://files.readme.io/314a341e4efcff60b16c13c2d9d89c91866ff0a66eb39458de68670c9d1d7ab7-Screen_Recording_Jun_4_2025_from_Google_Drive_1.mp4_1.gif" alt="Manual Carousel Template" align="center" width="35% " caption="Manual Carousel Template" border={true} />


## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "right" }}>
        Manual Carousel Template Keys
      </th>

      <th style={{ textAlign: "center" }}>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_id
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Indicates the type of template, for example,`pt_manual_carousel`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_summary
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_subtitle
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl1
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Deep Link One
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl2
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link Two
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl`n`
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link for the nth image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img1
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image One
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img1\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image One
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img2
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image Two
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img2\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image Two
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img3
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image Three
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img3\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image Three
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_img`n`
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Image `N`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt _img_`n`alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image `N`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_ico
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        wzrk\_hide\_large\_icon
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications<br />Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_sticky
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dismiss
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_scale\_type
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li><li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_json
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_manual\_carousel\_type
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        `filmstrip`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif\_frames
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>
  </tbody>
</Table>

# Five Icons Template

The Five Icons template is a sticky push notification with no text, just five icons, and a close button that can help your users go directly to the functionality of their choice with a button click.

The library does not notify if at least three icons are not retrieved. The bifurcation of each CTA is captured in the event Notification Clicked within the property `wzrk_c2a`.

By default, if the user clicks on any notification area except the five and close icons, an activity intent launches.


<Image src="https://files.readme.io/563c039e56f135aea167f9fecda45c428087ada312e0013d1347567fcd8f724e-image.png" align="center" width="50% " border={true} />


## Template Keys

|   Five Icons Template Keys | Required | Description                                                       |
| -------------------------: | :------: | :---------------------------------------------------------------- |
|                     pt\_id | Required | Indicates the type of template, for example,`pt_five_icons`       |
|                   pt\_img1 | Required | Icon One                                                          |
|        pt\_img1\_alt\_text | Optional | Alt Text for Icon One                                             |
|                   pt\_img2 | Required | Icon Two                                                          |
|        pt\_img2\_alt\_text | Optional | Alt Text for Icon Two                                             |
|                   pt\_img3 | Required | Icon Three                                                        |
|        pt\_img3\_alt\_text | Optional | Alt Text for Icon Three                                           |
|                   pt\_img4 | Optional | Icon Four                                                         |
|        pt\_img4\_alt\_text | Optional | Alt Text for Icon Four                                            |
|                   pt\_img5 | Optional | Icon Five                                                         |
|        pt\_img5\_alt\_text | Optional | Alt Text for Icon Five                                            |
|                    pt\_dl1 | Required | Deep Link for first icon                                          |
|                    pt\_dl2 | Required | Deep Link for second icon                                         |
|                    pt\_dl3 | Required | Deep Link for third icon                                          |
|                    pt\_dl4 | Optional | Deep Link for fourth icon                                         |
|                    pt\_dl5 | Optional | Deep Link for fifth icon                                          |
|                     pt\_bg | Required | Background Color in HEX                                           |
|       pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                                           |
| pt\_small\_icon\_clr\_dark | Optional | Small Icon Color in HEX in dark mode                              |
|                 pt\_sticky | Optional | Should the notification be sticky? ("true"/"false")               |
|                pt\_dismiss | Optional | Auto dismiss the notification after a set time (value in seconds) |
|                   pt\_json | Optional | Above keys in JSON format                                         |

# Timer Template

Timer template is a dynamic and time-sensitive push notification template designed to display a live countdown timer directly within the notification. This format is ideal for creating urgency around limited-time offers, flash sales, upcoming events, or deadlines. You can update the new message, title, or background image automatically once the countdown expires, keeping the notification relevant and engaging after expiry.

Timer notification is only supported for Android version 8 and above. For Android versions below 8, the library falls back to the _Basic Template_.


<Image src="https://files.readme.io/a0dd4a50b5b7783a1f970928b1d13d16d660728b9afb935e4947fbd3c942fb00-Timer_12_Light_Live.mp4_1.gif" alt="Timer Template" align="center" width="35% " caption="Timer Template" />


## Template Keys

|          Timer Template Keys | Required | Description                                                                                                                                                                                                                        |
| ---------------------------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                       pt\_id | Required | Indicates the type of template, for example, `pt_timer`                                                                                                                                                                            |
|                    pt\_title | Required | Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.                                                                            |
|               pt\_title\_alt | Optional | Title to show after timer expires                                                                                                                                                                                                  |
|                      pt\_msg | Required | Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.                                                                                                  |
|                 pt\_msg\_alt | Optional | Message to show after timer expires                                                                                                                                                                                                |
|             pt\_msg\_summary | Optional | Message line when Notification is expanded                                                                                                                                                                                         |
|                 pt\_subtitle | Optional | Subtitle                                                                                                                                                                                                                           |
|                      pt\_dl1 | Required | Deep Link                                                                                                                                                                                                                          |
|                 pt\_big\_img | Optional | Image                                                                                                                                                                                                                              |
|            pt\_big\_img\_alt | Optional | Image to show when timer expires                                                                                                                                                                                                   |
|      pt\_big\_img\_alt\_text | Optional | Alt Text for Image                                                                                                                                                                                                                 |
| pt\_big\_img\_alt\_alt\_text | Optional | Alt Text for Image to show when timer expires                                                                                                                                                                                      |
|                      pt\_gif | Optional | GIF                                                                                                                                                                                                                                |
|              pt\_gif\_frames | Optional | Number of frames to extract from the GIF                                                                                                                                                                                           |
|                       pt\_bg | Required | Background Color in HEX                                                                                                                                                                                                            |
|                 pt\_bg\_dark | Required | Background Color in HEX in dark mode                                                                                                                                                                                               |
|       pt\_chrono\_title\_clr | Optional | Color for timer text in HEX                                                                                                                                                                                                        |
| pt\_chrono\_title\_clr\_dark | Optional | Color for timer text in HEX in dark mode                                                                                                                                                                                           |
|         pt\_timer\_threshold | Required | Timer duration in seconds (minimum 10)                                                                                                                                                                                             |
|               pt\_timer\_end | Optional | Epoch Timestamp to countdown to (for example, $D\_1595871380 or 1595871380). Not required if `pt_timer_threshold` is specified.                                                                                                    |
|               pt\_title\_clr | Optional | Title Color in HEX                                                                                                                                                                                                                 |
|         pt\_title\_clr\_dark | Optional | Title Color in HEX in dark mode                                                                                                                                                                                                    |
|                 pt\_msg\_clr | Optional | Message Color in HEX                                                                                                                                                                                                               |
|           pt\_msg\_clr\_dark | Optional | Message Color in HEX in dark mode                                                                                                                                                                                                  |
|         pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                                                                                                                                                                                                            |
|   pt\_small\_icon\_clr\_dark | Optional | Small Icon Color in HEX in dark mode                                                                                                                                                                                               |
|              pt\_scale\_type | Optional | Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling). |
|         pt\_render\_terminal | Optional | Determines whether terminal notification is rendered<br />Value- `true` or `false`                                                                                                                                                 |
|                   pt\_sticky | Optional | Should the notification be sticky? ("true"/"false")                                                                                                                                                                                |
|                  pt\_dismiss | Optional | Auto dismiss the notification after a set time (value in seconds)                                                                                                                                                                  |
|        pt\_msg\_summary\_alt | Optional | Summary to display once the timer expires                                                                                                                                                                                          |
|                     pt\_json | Optional | Above keys in JSON format                                                                                                                                                                                                          |
|            pt\_chrono\_style | Optional | Controls styling mode. Available values: <code>solid</code> (default),  <code>gradient\_linear</code> ,  <code>gradient\_radial</code>                                                                                             |
|          pt\_chrono\_bg\_clr | Optional | Background color (HEX). Required if style = `solid`                                                                                                                                                                                |
|      pt\_chrono\_border\_clr | Optional | Border color (HEX). No border if absent                                                                                                                                                                                            |
|       pt\_chrono\_grad\_clr1 | Optional | Gradient start color (HEX)                                                                                                                                                                                                         |
|       pt\_chrono\_grad\_clr2 | Optional | Gradient end color (HEX)                                                                                                                                                                                                           |
|        pt\_chrono\_grad\_dir | Optional | Angle in degrees (String). Default <code>"90"</code>                                                                                                                                                                               |
|   pt\_chrono\_border\_radius | Optional | Corner radius in dp (Density-independent Pixels) (Default: 6)                                                                                                                                                                      |
|    pt\_chrono\_border\_width | Optional | Border width in dp. No border if absent, even if `pt_chrono_border_clr` is set.                                                                                                                                                    |
|       pt\_chrono\_title\_clr | Optional | Countdown text color (falls back to <code>pt\_title\_clr</code>)                                                                                                                                                                   |

<Callout icon="📘" theme="info">
  ## Note

  When a notification arrives with an already-expired timer (or remaining time below 10 seconds), and if `pt_render_terminal` is true, the SDK renders the terminal/fallback notification as a Basic template (using `pt_title_alt`, `pt_msg_alt`, `pt_big_img_alt`). If `pt_render_terminal` is false, the notification is silently skipped.
</Callout>

# Text over Image Template

Text over Image template is a visually immersive push notification format designed to maximize the use of imagery. In this layout, the background image spans the entire surface area of the notification, creating an edge-to-edge visual experience. All accompanying text such as the title, message, and call-to-action is seamlessly overlaid on top of the image, ensuring the message is prominent while maintaining a clean, modern aesthetic.

The library falls back to the Advanced Template if the image cannot be downloaded.


<Image src="https://files.readme.io/aca5987a4cff4a252167cf9c0c954a422b08bfd7cfa7860b6ebb2c7bd0bc40f9-image.png" alt="Text over Image Push Template" align="center" width="35% " caption="Text over Image Push Template" />


## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "right" }}>
        Text over Image Template Keys
      </th>

      <th style={{ textAlign: "center" }}>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_id
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_zero_bezel`.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_summary
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message line when Notification is expanded.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_subtitle
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif\_frames
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_view
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Select text-only small view layout (`text_only`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl1
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_scale\_type
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li><li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img\_collapsed
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Image for the collapsed view
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif\_collapsed
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        GIF for the collapsed view
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif\_frames\_collapsed
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF for the collapsed view
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img\_collapsed\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image in the collapsed view
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_scale\_type\_collapsed
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        ScaleType for the image in the collapsed View ("center\_crop"/"fit\_center")
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_sticky
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dismiss
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_ico
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        wzrk\_hide\_large\_icon
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications<br />Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_json
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Rating Template

Rating template lets your users give you feedback. This feedback is captured in the event _Rating Submitted_ within the property `wzrk_c2a`.


<Image src="https://files.readme.io/e026a6e56be3574972ff15ae648e46be20699f55cd77eac60f318c3d77c3948e-image.png" alt="Rating Template" align="center" width="35% " caption="Rating Template" />


## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "right" }}>
        Rating Template Keys
      </th>

      <th style={{ textAlign: "center" }}>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_id
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Value - `pt_rating`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Title
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Message
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_default\_dl
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Default Deep Link for Push Notification
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl1
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Deep Link for first/all star(s)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_bg\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_summary
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_subtitle
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl2
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link for second star
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl3
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link for third star
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl4
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link for fourth star
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl5
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Deep Link for fifth star
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_ico
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        wzrk\_hide\_large\_icon
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications<br />Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_scale\_type
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li><li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:push-templates-android#image-scaling).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_json
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img\_alt\_text
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_gif\_frames
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_sticky
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dismiss
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>
  </tbody>
</Table>

# Product Catalog Template

The product catalog template lets you showcase different product images (or a product catalog) before the user can click on the _BUY NOW_ option, which can take them directly to the product via deep links. This template has two variants.

## Vertical View


<Image src="https://files.readme.io/21393452affbcdbd00b8a7550c07b59db3a855a46f0746099529338ef5208a2f-image.png" alt="Product Catalog Template- Vertical View" align="center" width="35% " caption="Product Catalog Template- Vertical View" border={true} />


## Linear View

Use the following keys to enable the linear view variant of this template:

|                 Template Key | Required | Value  |
| ---------------------------: | :------: | :----- |
| pt\_product\_display\_linear | Optional | `true` |


<Image src="https://files.readme.io/1e2e81891b1ce9faa0a12731f254d616dcf51818c28f14f6428c0a5d833855b8-image.png" alt="Product Catalog Template-Linear View" align="center" width="35% " caption="Product Catalog Template-Linear View" border={true} />


## Template Keys

|     Product Catalog Template Keys | Required | Description                                                                                                                                                                                                                        |
| --------------------------------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                            pt\_id | Required | Value - `pt_product_display`                                                                                                                                                                                                       |
|                           pt\_msg | Required | Message                                                                                                                                                                                                                            |
|                         pt\_title | Required | Title                                                                                                                                                                                                                              |
|                          pt\_img1 | Required | Image One                                                                                                                                                                                                                          |
|               pt\_img1\_alt\_text | Optional | Alt Text for Image One                                                                                                                                                                                                             |
|                          pt\_img2 | Required | Image Two                                                                                                                                                                                                                          |
|               pt\_img2\_alt\_text | Optional | Alt Text for Image Two                                                                                                                                                                                                             |
|                          pt\_img3 | Required | Image Three                                                                                                                                                                                                                        |
|               pt\_img3\_alt\_text | Optional | Alt Text for Image Three                                                                                                                                                                                                           |
|                           pt\_bt1 | Required | Big text for first image                                                                                                                                                                                                           |
|                           pt\_bt2 | Required | Big text for second image                                                                                                                                                                                                          |
|                           pt\_bt3 | Required | Big text for third image                                                                                                                                                                                                           |
|                           pt\_st2 | Required | Small text for second image                                                                                                                                                                                                        |
|                           pt\_st1 | Required | Small text for first image                                                                                                                                                                                                         |
|                           pt\_st3 | Required | Small text for third image                                                                                                                                                                                                         |
|                           pt\_dl1 | Required | Deep Link for first image                                                                                                                                                                                                          |
|                           pt\_dl2 | Required | Deep Link for second image                                                                                                                                                                                                         |
|                           pt\_dl3 | Required | Deep Link for third image                                                                                                                                                                                                          |
|                        pt\_price1 | Required | Price for first image                                                                                                                                                                                                              |
|                        pt\_price2 | Required | Price for second image                                                                                                                                                                                                             |
|                        pt\_price3 | Required | Price for third image                                                                                                                                                                                                              |
|                            pt\_bg | Required | Background Color in HEX                                                                                                                                                                                                            |
|                      pt\_bg\_dark | Required | Background Color in HEX in dark mode                                                                                                                                                                                               |
|      pt\_product\_display\_action | Required | Action Button Label Text                                                                                                                                                                                                           |
| pt\_product\_display\_action\_clr | Required | Action Button Background Color in HEX                                                                                                                                                                                              |
|                      pt\_subtitle | Optional | Subtitle                                                                                                                                                                                                                           |
|      pt\_product\_display\_linear | Optional | Linear Layout Template ("true"/"false")                                                                                                                                                                                            |
|                    pt\_title\_clr | Optional | Title Color in HEX                                                                                                                                                                                                                 |
|              pt\_title\_clr\_dark | Optional | Title Color in HEX in dark mode.                                                                                                                                                                                                   |
|                      pt\_msg\_clr | Optional | Message Color in HEX                                                                                                                                                                                                               |
|                pt\_msg\_clr\_dark | Optional | Message Color in HEX in dark mode                                                                                                                                                                                                  |
|              pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                                                                                                                                                                                                            |
|        pt\_small\_icon\_clr\_dark | Optional | Small Icon Color in HEX in dark mode                                                                                                                                                                                               |
|                        pt\_sticky | Optional | Should the notification be sticky? ("true"/"false")                                                                                                                                                                                |
|                       pt\_dismiss | Optional | Auto dismiss the notification after a set time (value in seconds)                                                                                                                                                                  |
|                   pt\_scale\_type | Optional | Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li><br />For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling). |
|                          pt\_json | Optional | Above keys in JSON format                                                                                                                                                                                                          |

# Input Box Template

The Input Box Template lets you collect any input, including feedback from your users. It has four variants.

## With CTAs

The CTA variant of the Input Box Template uses action buttons on the notification to collect input from the user.

To set the CTAs, use the Advanced Options when setting up the campaign on the dashboard.


<Image src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputctabasicdismiss.gif" alt="618" align="center" width="35% " caption="Input Box Template-With CTAs" border={true} />


|           Template Key | Required | Value                                                        |
| ---------------------: | :------: | :----------------------------------------------------------- |
| pt\_dismiss\_on\_click | Optional | `true` (Dismisses the notification without opening the app). |

<Callout icon="📘" theme="info">
  ### **Display the Notification in the tray (for Android 12 and above)**

  To keep the notification in the notification tray even after clicking the CTA, set the value for the `pt_dismiss_on_click` payload key as `false` and add the `dismissNotification` function to your code.
</Callout>

Following is a sample code for adding the `dismissNotification` function:

```kotlin
fun dismissNotification(intent: Intent?, applicationContext: Context){
        intent?.extras?.apply {
            var autoCancel = true
            var notificationId = -1

            getString("actionId")?.let {
                Log.d("ACTION_ID", it)
                autoCancel = getBoolean("autoCancel", true)
                notificationId = getInt("notificationId", -1)
            }
            /**
             * If using InputBox template, add ptDismissOnClick flag to not dismiss notification
             * if pt_dismiss_on_click is false in InputBox template payload. Alternatively if normal
             * notification is raised then we dismiss notification.
             */
            val ptDismissOnClick = intent.extras!!.getString(PTConstants.PT_DISMISS_ON_CLICK,"")

            if (autoCancel && notificationId > -1 && ptDismissOnClick.isNullOrEmpty()) {
                val notifyMgr: NotificationManager =
                    applicationContext.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
                notifyMgr.cancel(notificationId)
            }
        }
    }
```

## CTAs with Remind Later option

This variant of the Input Box Template is advantageous if the user wants to be reminded of the notification after some time. Clicking on the remind later button raises an event to the user profiles, with a custom user property p2, whose value is a future timestamp. You can have a campaign running on the dashboard that will send a reminder notification at the timestamp in the event property.

Ensure to set one of the CTAs as a Remind Later button. Set the action ID to `remind` from the dashboard.

|                                      Template Key | Required | Value                                                                                                                                                                  |
| ------------------------------------------------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                                   pt\_event\_name | Required | For example: `Remind Later`,                                                                                                                                           |
| pt\_even&#x74;_&#x70;roperty_\<property\_name\_1> | Optional | For example: `<property_value>`,                                                                                                                                       |
| pt\_even&#x74;_&#x70;roperty_\<property\_name\_2> | Optional | future epoch timestamp. For example: `\$D_1592503813`                                                                                                                  |
|                            pt\_dismiss\_on\_click | Required | Value should be `true`. It dismisses the notification without opening the app and raises a required event to the user profile, needed to send a reminder notification. |


<Image src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaRemind.gif" alt="618" align="center" width="35% " caption="Input Box Template-CTAs with Remind Later option" border={true} />


## Reply as an Event

This variant raises an event capturing the user's input as an event property. The app is not opened after the user sends the reply.

To use this variant, use the following values for the keys.

|                                      Template Key | Required | Value                                   |
| ------------------------------------------------: | :------: | :-------------------------------------- |
|                                  pt\_input\_label | Required | for example, `Search`                   |
|                               pt\_input\_feedback | Required | for example, `Thanks for your feedback` |
|                                   pt\_event\_name | Required | for example. `Searched`,                |
| pt\_even&#x74;_&#x70;roperty_\<property\_name\_2> | Required | fixed value - `pt_input_reply`          |
| pt\_even&#x74;_&#x70;roperty_\<property\_name\_1> | Optional | for example. `<property_value>`,        |


<Image src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaNoOpen.gif" alt="618" align="center" width="35% " caption="Input Box Template-Reply as an Event" border={true} />


## Reply as an Intent

This variant passes the reply to the app as an Intent. The app can then process the reply and take appropriate actions.

To use this variant, use the following values for the keys.

|          Template Key | Required | Value                                    |
| --------------------: | :------: | :--------------------------------------- |
|      pt\_input\_label | Required | for example, `Search`                    |
|   pt\_input\_feedback | Required | for example., `Thanks for your feedback` |
| pt\_input\_auto\_open | Required | fixed value - `true`                     |

To capture the input, the app can get the `pt_input_reply` key from the Intent extras.


<Image src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaWithOpen.gif" alt="618" align="center" width="35% " caption="Input Box Template-Reply as an Intent" border={true} />


## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "right" }}>
        Input Box Template Keys
      </th>

      <th style={{ textAlign: "center" }}>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_id
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Value - `pt_input`
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Title
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Message
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_big\_img\_alt
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Image to be shown after feedback is collected
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_input\_label
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Label text to be shown on the input
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_input\_feedback
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Feedback
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dl1
      </td>

      <td style={{ textAlign: "center" }}>
        Required
      </td>

      <td>
        Deep Link
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_summary
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_subtitle
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_event\_name
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Name of Event to be raised
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_even&#x74;_&#x70;roperty_\<property\_name\_1>
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Value for event property \<property\_name\_1>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_even&#x74;_&#x70;roperty_\<property\_name\_2>
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Value for event property \<property\_name\_2>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_even&#x74;_&#x70;roperty_\<property\_name\_n>
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Value for event property \<property\_name\_n>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_input\_auto\_open
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Auto open the app after feedback
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_title\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_msg\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_small\_icon\_clr\_dark
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_ico
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        wzrk\_hide\_large\_icon
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications<br />Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_dismiss\_on\_click
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Dismiss notification on click
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "right" }}>
        pt\_json
      </td>

      <td style={{ textAlign: "center" }}>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Vertical Image with CTA Template

The Vertical Image with CTA template is designed for displaying portrait-oriented creatives without cropping or resizing. It preserves the original layout of vertical assets, making it ideal for campaigns such as product showcases, posters, or reel-style creatives.

This template ensures that vertical-first designs appear exactly as intended, reducing the need to redesign assets for square or landscape formats. It supports expanded and collapsed views on Android. You can also include additional text fields for richer layouts and an optional CTA button with customizable styles.

<Callout icon="📘" theme="info">
  ### Private Beta

  Vertical Image with CTA is available in Private Beta. To enable this feature, contact your Customer Success Manager.
</Callout>


<Image src="https://files.readme.io/6170b638867cb0457a6744586cdf70a8df6250759cb0b828c3c90923bbb66d95-ChatGPT_Image_Apr_20_2026_at_12_27_11_PM.png" align="center" width="35% " caption="Vertical Image with CTA Template" border={true} />


<Callout icon="📘" theme="info">
  ### Note

  The minimum SDK version required for the Vertical Image with CTA Template is Push Templates SDK v2.4.0.
</Callout>

# GIF Support

CleverTap supports GIFs in push templates starting with SDK version 7.5.0 on devices running Android 14 or later. This allows you to add animated media to push notifications, enhancing user engagement through dynamic visuals.


<Image src="https://files.readme.io/47c076dffe3d89c693a3fc4aeac5117649dc0a28af5e2b820157f86d7919ed29-Screen_Recording_Jul_28_2025_1.gif" align="center" width="35% " />


## Supported Templates

GIFs are supported in the following templates:

| Template Name            | Key Parameter | Supported From | Notes                                                                         |
| :----------------------- | :------------ | :------------- | :---------------------------------------------------------------------------- |
| Standard Template        | `wzrk_gif`    | SDK v7.5.0     | Android 14+ only; size limits below do **not apply**                          |
| Advanced Template        | `pt_gif`      | SDK v7.5.0     | Supports optional `pt_gif_frames`                                             |
| Timer Template           | `pt_gif`      | SDK v7.5.0     | GIF continues until timer expiry                                              |
| Text over Image Template | `pt_gif`      | SDK v7.5.0     | Supports `pt_gif` for expanded and `pt_gif_collapsed` for collapsed view      |
| Rating Template          | `pt_gif`      | SDK v7.5.0     | Optional; static fallback if unsupported                                      |
| Vertical Image with CTA  | `pt_gif`      | SDK v7.5.0     | Supports GIFs in expanded (`pt_gif`) and collapsed (`pt_gif_collapsed`) views |

<Callout icon="📘" theme="info">
  ### Note:

  For templates other than _Standard Push Template_, Refer to the [GIF Size Guidelines](doc:android-push-templates-v2#gif-size-guidelines) for smooth rendering.
</Callout>

## GIF Size Guidelines

To ensure optimal performance across devices:

| GIF Size     | Recommended Number of Frames |
| :----------- | :--------------------------- |
| 180 x 120 px | 15                           |
| 240 x 160 px | 10                           |
| 300 x 200 px | 6                            |

You can use [Ezgif Tool](https://ezgif.com/resize) to resize or optimize your GIFs.

## Example: Standard Push Template with GIF

To use a GIF in a Standard Push Template, include the following payload key:

```json
{
  "wzrk_gif": "https://example.com/your-gif-url.gif"
}
```

For templates using `pt_gif`, specify the key as:

```json
{
  "pt_gif": "https://example.com/your-gif-url.gif"
}
```

### SDK Requirements

| Requirement           | Minimum Version                     |
| --------------------- | ----------------------------------- |
| CleverTap Android SDK | v7.5.0                              |
| Push Templates SDK    | v2.2.0                              |
| Android OS            | 14 and above for animation playback |

## Limitations

- Devices running Android 13 and below do not support GIF playback; a static fallback image is shown.
- Some OEMs may not fully support GIF rendering, even on Android 14 and above.
- On certain devices, the heads-up notification may display only the first frame, while the full GIF appears when the notification tray is expanded.
- GIFs may not loop smoothly if the frame count is high or the file size exceeds 1 MB.

# Template Variations

Users can preview and render different variations of outputs for each Push template type based on the following factors:

- **Version-based Output**: Notifications render differently for Android version 12 and above and version 11 and below. Key considerations include the following:
  - **Android 13 and above**: Use images no larger than 450x450 px for optimal rendering.
  - **Android 12 and above**: Center-align any text within images is centered for to ensure better visibility.
  - **Android 11 and 12**: Use image file size below 100 KB to prevent rendering issues.
  - **Android 11 and below**: Use images of under 3 MB or lower is recommended for better to maintain optimal performance.
- **CTA-based Output**: Notification renders based on whether CTA buttons are included in the payload.

# Dark Mode

Push templates automatically adapt to the device's theme settings, rendering notifications in dark or light mode.

| **Theme Setting**      | **Notification Appearance**                       | Sample Output                                                                                           |
| ---------------------- | ------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| **Dark mode enabled**  | Notification renders with a **dark background**   | ![](https://files.readme.io/37884bc08c5375da82457ffc4f54c7b31d3dc1591e0430f65949b73003df5e35-image.png) |
| **Dark mode disabled** | Notification renders with a **light background**. | ![](https://files.readme.io/e5e9256facd596cdea7b325ad9945868ed3a4d3c5527ef09d46ae64ae350fc5d-image.png) |

<Callout icon="📘" theme="info">
  ### **Custom Color Definitions**

  Templates support **custom color definitions** that adapt to both themes for visual consistency.
</Callout>

<Callout icon="🚧" theme="warn">
  ### **SDK Version Requirement**

  To use the Dark mode feature, ensure you are using Push Templates SDK version 2.1.0 and above.
</Callout>

# Image Scaling

Android supports various image scaling options to control how images appear in push notifications. CleverTap optimizes image rendering to maintain visual consistency across devices while leveraging Android native scaling behavior. To handle scaling in a Push template, you must add the key `pt_scale_type` key and the value is set as `fit_center` or `center_crop` based on the requirement.

The following table lists the different scaling settings in Push templates:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Image Scaling Type
      </th>

      <th>
        Description
      </th>

      <th>
        Sample Output
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **Keep Original**
      </td>

      <td>
        Applies to all expanded images in Android push templates.

        The image is scaled proportionally (maintaining the aspect ratio) to ensure that both its width and height fit within the boundaries of the image container. This ensures the entire image is visible without any cropping.
      </td>

      <td>
        ![](https://files.readme.io/8ee6f3ee4c83f0c5858268cca7d494fdc6492c2eaff59a7daf85d504260f365c-image.png)
      </td>
    </tr>

    <tr>
      <td>
        **Scale to Fill**
      </td>

      <td>
        Applies to all expanded images in all Android push templates.

        The image is resized uniformly while preserving the aspect ratio.  If necessary, it is scaled down so that both width and height fit within the image container. This ensures the full image is displayed without any part being cropped.
      </td>

      <td>
        ![](https://files.readme.io/ca12801ffa07a814fa59f413d383b80185acb5d6c9dc36b5aa04721bc21800ae-image.png)
      </td>
    </tr>
  </tbody>
</Table>

<Callout icon="🚧" theme="warn">
  ### **SDK Version Requirement**

  To use the Image scaling feature, ensure you are using Push Templates SDK version 2.1.0 and above.
</Callout>

# Image Specifications

The following table shows image specifications, including both standard aspect ratios and CTA-specific variations. Aspect ratios may differ when Call-to-Action buttons are included in the payload.

| Template                     | OS Version         | Aspect Ratio (Without CTA) | Aspect Ratio (With CTA) | Max File Size |
| :--------------------------- | :----------------- | :------------------------- | :---------------------- | :------------ |
| Standard                     | Android 11 & below | 5:3                        | 2:1                     | -             |
|                              | Android 12+        | 3:2                        | 2:1                     | -             |
| Advanced                     | Android 11 & below | 5:3                        | 2:1                     | 500 KB        |
|                              | Android 12+        | 3:2                        | 2:1                     | 500 KB        |
| Auto Carousel                | Android 11 & below | 5:3                        | 2:1                     | 86 KB         |
|                              | Android 12+        | 3:2                        | 2:1                     | 86 KB         |
| Manual Carousel              | Android 11 & below | 5:3                        | 2:1                     | 86 KB         |
|                              | Android 12+        | 3:2                        | 2:1                     | 86 KB         |
| Five Icon                    | Android 11 & below | 1:1                        | N/A                     | 50 KB         |
|                              | Android 12+        | 1:1                        | N/A                     | 50 KB         |
| Text over Image              | Android 11 & below | 2:1                        | 4:3                     | 500 KB        |
|                              | Android 12+        | 1:1                        | 4:3                     | 500 KB        |
| Timer                        | Android 11 & below | 5:3                        | 2:1                     | 326 KB        |
|                              | Android 12+        | 3:2                        | 2:1                     | 326 KB        |
| Rating                       | Android 11 & below | 5:3                        | N/A                     | 500 KB        |
|                              | Android 12+        | 3:2                        | N/A                     | 500 KB        |
| Product Catalogue (Linear)   | Android 11 & below | 2:1                        | N/A                     | 86 KB         |
|                              | Android 12+        | 3:2                        | N/A                     | 86 KB         |
| Product Catalogue (Vertical) | Android 11 & below | 1:1                        | N/A                     | 86 KB         |
|                              | Android 12+        | 3:4                        | N/A                     | 86 KB         |
| Input Box                    | Android 11 & below | 5:3                        | N/A                     | 500 KB        |
|                              | Android 12+        | 3:2                        | N/A                     | 500 KB        |
| Vertical Image with CTA      | Android 11 & below | 2:3                        | 2:3                     | 500 KB        |
|                              | Android 12+        | 2:3                        | 2:3                     | 500 KB        |

# Image Guidelines

Ensure images for the following templates meet the specified resolution and file size guidelines:

| Template Name              | Recommended Resolution | Additional Guidelines                                                                            |
| -------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------ |
| Standard Template          | -                      | No specific guidelines; single image template                                                    |
| Auto Carousel Template     | 400 x 300 px           | File size per image: 86 KB max                                                                   |
| Manual Carousel Template   | 240 x 180 px           | File size per image: 86 KB max                                                                   |
| Five Icon Template         | 300 x 300 px           | Square aspect ratio (1:1); 5 icons total; file size per icon: 50 KB max                          |
| Product Catalogue Template | 225 x 225 px           | File size per product: 86 KB max; Vertical variant uses 3:4 aspect ratio (Android 12+)           |
| Standard Template with GIF | 240 x 140 px           | 5 frames max; Android 14+ recommended; size limits do not apply to Standard template GIFs        |
| Text over Image Template   | -                      | Center-align text within image for Android 12+; no specific guidelines for single image template |

<Callout icon="📘" theme="info">
  ### Note

  - These are recommended aspect ratios and file size limits.
  - Number of lines of text in `msg_summary` affects how an image renders for a particular aspect ratio. Adjust aspect ratios and layout accordingly based on message length.
  - Resolution limits do not change based on whether the push has CTA buttons or not.
  - Templates other than Auto Carousel, Manual Carousel, and Five Icons do not require specific image resolution guidelines as they use a single image.
</Callout>

# FAQs

For FAQs, refer to [Push Templates FAQs](https://docs.clevertap.com/docs/troubleshooting-faqs-push-notifications).

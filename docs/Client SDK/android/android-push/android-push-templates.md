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

* [Out-of-the-box Implementation](doc:android-push-templates#configure-out-of-the-box-push-notification-handling) : Uses CleverTap's default notification handling, where templates are rendered without additional customization or code changes in your app.
* [Custom Implementation](doc:android-push-templates#customize-push-notification-handling): Allows you to control how push notifications are received and displayed.

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

* [Standard Push Template](doc:android-push-templates#standard-push-template)
* [Advanced Push Template](doc:android-push-templates#advanced-push-template)

**Carousel Templates**

* [Auto Carousel Template](doc:android-push-templates#auto-carousel-template)
* [Manual Carousel Template](doc:android-push-templates#manual-carousel-template)

**Advanced Templates**

* [Timer Template](doc:android-push-templates#timer-template)
* [Text over Image Template](doc:android-push-templates#text-over-image-template)
* [Five Icons Template](doc:android-push-templates#five-icons-template)
* [Rating Template](doc:android-push-templates#rating-template)
* [Product Catalog Template](doc:android-push-templates#product-catalog-template)
* [Input Box Template](doc:android-push-templates#input-box-template)

# Standard Push Template

Standard Template is a default template for push notifications received on apps that supports a simple layout with title, message, image, and optional buttons, ideal for quick, informative alerts. For example, an e-commerce business might want to inform customers that their order is on the way. To use a GIF in a standard push template, pass the key `wzrk_gif` with the value set to the URL of the GIF image.

<Image align="center" alt="Standard Push Template" border={false} caption="Standard Push Template" src="https://files.readme.io/7a4e66924248b609d31ccdc200b05788c5a2b1b5e6550e58de559233caefe4b2-image.png" width="35% " />

## GIF Support in Standard Push Template

CleverTap supports GIFs in standard push templates from SDK version 7.5.0 on devices running Android 14 and above. This allows you to add animated media to push notifications, enhancing user engagement through dynamic visuals.

<Image align="center" border={false} width="35% " src="https://files.readme.io/47c076dffe3d89c693a3fc4aeac5117649dc0a28af5e2b820157f86d7919ed29-Screen_Recording_Jul_28_2025_1.gif" />

### Specifications and Design Guidelines

* Supported on Android 14+ with SDK v7.5.0 or later.
* Applies only to the Standard push template.
* For older Android versions or SDKs, CleverTap shows the first frame of the GIF as a static image.
* Use the recommended aspect ratio for the selected template to avoid cropping or distortion.
* Keep file size under 1MB for smooth rendering.
* Opt for looped, visually clear GIFs that work well even if partially cropped.

### Limitations

* Devices running Android 13 and below do not support GIF playback; a static fallback image is shown.
* Some OEMs may not fully support GIF rendering, even on Android 14 and above.
* On certain devices, the heads-up notification may display only the first frame, while the full GIF appears when the notification tray is expanded.

# Advanced Push Template

This template supports rich media, deep links, and enhanced customization for more engaging messages. It is ideal for interactive, visually appealing messages. For example, businesses can use it to build excitement for new product drops, early access offers, or feature rollouts that benefit from strong visuals and clear Call to Action (CTAs).

For template outputs Android 11 and below, refer to [Push Templates on Android v11 and below](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESANDROID12.md).

<Image align="center" alt="Advanced Push Template" border={false} caption="Advanced Push Template" src="https://files.readme.io/65cc6bcd2523518ceab5cdc452022fbc4f4bce889e87469c883fd394b1fb18e4-image.png" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Advanced Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_basic`.
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Required
      </td>

      <td>
        Message line when Notification is expanded.
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_bg
      </td>

      <td>
        Required
      </td>

      <td>
        Background color of your push notification. You can use it to match your brand theme or draw visual attention to the notification. It accepts HEX codes or color pickers.
      </td>
    </tr>

    <tr>
      <td>
        pt_bg_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Background color of your push notification in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img
      </td>

      <td>
        Optional
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td>
        pt_gif
      </td>

      <td>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_frames
      </td>

      <td>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_ico
      </td>

      <td>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td>
        wzrk_hide_large_icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications  
        Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Optional
      </td>

      <td>
        One Deep Link (minimum)
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
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

<Image align="center" alt="Auto Carousel Template" border={false} caption="Auto Carousel Template" src="https://files.readme.io/b06f01f282ddb61ab6bca44b51c79e65c9735b224432fadaf0799eb6c1dd7a70-Screen_Recording_Jun_4_2025_from_Google_Drive.mp4_1.gif" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Auto Carousel Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_carousel`.
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Optional
      </td>

      <td>
        Message line when Notification is expanded.
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link (Max one)
      </td>
    </tr>

    <tr>
      <td>
        pt_img1
      </td>

      <td>
        Required
      </td>

      <td>
        Image One
      </td>
    </tr>

    <tr>
      <td>
        pt_img1_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image One
      </td>
    </tr>

    <tr>
      <td>
        pt_img2
      </td>

      <td>
        Required
      </td>

      <td>
        Image Two
      </td>
    </tr>

    <tr>
      <td>
        pt_img2_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image Two
      </td>
    </tr>

    <tr>
      <td>
        pt_img3
      </td>

      <td>
        Required
      </td>

      <td>
        Image Three
      </td>
    </tr>

    <tr>
      <td>
        pt_img3_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image Three
      </td>
    </tr>

    <tr>
      <td>
        pt_img`n`
      </td>

      <td>
        Optional
      </td>

      <td>
        Image `N`
      </td>
    </tr>

    <tr>
      <td>
        pt_img`n`_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image `N`
      </td>
    </tr>

    <tr>
      <td>
        pt_bg
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_bg_dark
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_ico
      </td>

      <td>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td>
        wzrk_hide_large_icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications  
        Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
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

<Image align="center" alt="Manual Carousel Template" border={true} caption="Manual Carousel Template" src="https://files.readme.io/314a341e4efcff60b16c13c2d9d89c91866ff0a66eb39458de68670c9d1d7ab7-Screen_Recording_Jun_4_2025_from_Google_Drive_1.mp4_1.gif" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Manual Carousel Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Indicates the type of template, for example,`pt_manual_carousel`
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link One
      </td>
    </tr>

    <tr>
      <td>
        pt_dl2
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link Two
      </td>
    </tr>

    <tr>
      <td>
        pt_dl`n`
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link for the nth image
      </td>
    </tr>

    <tr>
      <td>
        pt_img1
      </td>

      <td>
        Required
      </td>

      <td>
        Image One
      </td>
    </tr>

    <tr>
      <td>
        pt_img1_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image One
      </td>
    </tr>

    <tr>
      <td>
        pt_img2
      </td>

      <td>
        Required
      </td>

      <td>
        Image Two
      </td>
    </tr>

    <tr>
      <td>
        pt_img2_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image Two
      </td>
    </tr>

    <tr>
      <td>
        pt_img3
      </td>

      <td>
        Required
      </td>

      <td>
        Image Three
      </td>
    </tr>

    <tr>
      <td>
        pt_img3_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image Three
      </td>
    </tr>

    <tr>
      <td>
        pt_img`n`
      </td>

      <td>
        Optional
      </td>

      <td>
        Image `N`
      </td>
    </tr>

    <tr>
      <td>
        pt_img`n`_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image `N`
      </td>
    </tr>

    <tr>
      <td>
        pt_bg
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_bg_dark
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_ico
      </td>

      <td>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td>
        wzrk_hide_large_icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications  
        Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>

    <tr>
      <td>
        pt_manual_carousel_type
      </td>

      <td>
        Optional
      </td>

      <td>
        `filmstrip`
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_frames
      </td>

      <td>
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

<Image align="center" border={true} width="50% " src="https://files.readme.io/563c039e56f135aea167f9fecda45c428087ada312e0013d1347567fcd8f724e-image.png" className="border" />

## Template Keys

| Five Icons Template Keys | Required | Description                                                       |
| -----------------------: | :------: | :---------------------------------------------------------------- |
|                    pt_id | Required | Indicates the type of template, for example,`pt_five_icons`       |
|                  pt_img1 | Required | Icon One                                                          |
|         pt_img1_alt_text | Optional | Alt Text for Icon One                                             |
|                  pt_img2 | Required | Icon Two                                                          |
|         pt_img2_alt_text | Optional | Alt Text for Icon Two                                             |
|                  pt_img3 | Required | Icon Three                                                        |
|         pt_img3_alt_text | Optional | Alt Text for Icon Three                                           |
|                  pt_img4 | Optional | Icon Four                                                         |
|         pt_img4_alt_text | Optional | Alt Text for Icon Four                                            |
|                  pt_img5 | Optional | Icon Five                                                         |
|         pt_img5_alt_text | Optional | Alt Text for Icon Five                                            |
|                   pt_dl1 | Required | Deep Link for first icon                                          |
|                   pt_dl2 | Required | Deep Link for second icon                                         |
|                   pt_dl3 | Required | Deep Link for third icon                                          |
|                   pt_dl4 | Optional | Deep Link for fourth icon                                         |
|                   pt_dl5 | Optional | Deep Link for fifth icon                                          |
|                    pt_bg | Required | Background Color in HEX                                           |
|        pt_small_icon_clr | Optional | Small Icon Color in HEX                                           |
|   pt_small_icon_clr_dark | Optional | Small Icon Color in HEX in dark mode                              |
|                pt_sticky | Optional | Should the notification be sticky? ("true"/"false")               |
|               pt_dismiss | Optional | Auto dismiss the notification after a set time (value in seconds) |
|                  pt_json | Optional | Above keys in JSON format                                         |

# Timer Template

Timer template is a dynamic and time-sensitive push notification template designed to display a live countdown timer directly within the notification. This format is ideal for creating urgency around limited-time offers, flash sales, upcoming events, or deadlines. You can update the new message, title, or background image automatically once the countdown expires, keeping the notification relevant and engaging after expiry.

Timer notification is only supported for Android version 8 and above. For Android versions below 8, the library falls back to the _Basic Template_.

<Image align="center" alt="Timer Template" border={false} caption="Timer Template" src="https://files.readme.io/a0dd4a50b5b7783a1f970928b1d13d16d660728b9afb935e4947fbd3c942fb00-Timer_12_Light_Live.mp4_1.gif" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Timer Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_timer`
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_title_alt
      </td>

      <td>
        Optional
      </td>

      <td>
        Title to show after timer expires
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_alt
      </td>

      <td>
        Optional
      </td>

      <td>
        Message to show after timer expires
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img
      </td>

      <td>
        Optional
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt
      </td>

      <td>
        Optional
      </td>

      <td>
        Image to show when timer expires
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image to show when timer expires
      </td>
    </tr>

    <tr>
      <td>
        pt_gif
      </td>

      <td>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_frames
      </td>

      <td>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_bg
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_bg_dark
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_chrono_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Color for timer text in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_chrono_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Color for timer text in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_timer_threshold
      </td>

      <td>
        Required
      </td>

      <td>
        Timer duration in seconds (minimum 10)
      </td>
    </tr>

    <tr>
      <td>
        pt_timer_end
      </td>

      <td>
        Optional
      </td>

      <td>
        Epoch Timestamp to countdown to (for example, $D_1595871380 or 1595871380). Not required if `pt_timer_threshold` is specified.
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_render_terminal
      </td>

      <td>
        Optional
      </td>

      <td>
        Determines whether terminal notification is rendered  
        Value- `true` or `false`
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary_alt
      </td>

      <td>
        Optional
      </td>

      <td>
        Summary to display once the timer expires
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Text over Image Template

Text over Image template is a visually immersive push notification format designed to maximize the use of imagery. In this layout, the background image spans the entire surface area of the notification, creating an edge-to-edge visual experience. All accompanying text such as the title, message, and call-to-action is seamlessly overlaid on top of the image, ensuring the message is prominent while maintaining a clean, modern aesthetic.

The library falls back to the Advanced Template if the image cannot be downloaded.

<Image align="center" alt="Text over Image Push Template" border={false} caption="Text over Image Push Template" src="https://files.readme.io/aca5987a4cff4a252167cf9c0c954a422b08bfd7cfa7860b6ebb2c7bd0bc40f9-image.png" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Text over Image Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Indicates the type of template, for example, `pt_zero_bezel`.
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Optional
      </td>

      <td>
        Message line when Notification is expanded.
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img
      </td>

      <td>
        Required
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td>
        pt_gif
      </td>

      <td>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_frames
      </td>

      <td>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_small_view
      </td>

      <td>
        Optional
      </td>

      <td>
        Select text-only small view layout (`text_only`)
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_collapsed
      </td>

      <td>
        Optional
      </td>

      <td>
        Image for the collapsed view
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_collapsed
      </td>

      <td>
        Optional
      </td>

      <td>
        GIF for the collapsed view
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_frames_collapsed
      </td>

      <td>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF for the collapsed view
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_collapsed_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image in the collapsed view
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type_collapsed
      </td>

      <td>
        Optional
      </td>

      <td>
        ScaleType for the image in the collapsed View ("center_crop"/"fit_center")
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td>
        pt_ico
      </td>

      <td>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td>
        wzrk_hide_large_icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications  
        Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
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

<Image align="center" alt="Rating Template" border={false} caption="Rating Template" src="https://files.readme.io/e026a6e56be3574972ff15ae648e46be20699f55cd77eac60f318c3d77c3948e-image.png" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Rating Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Value - `pt_rating`
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Title
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Message
      </td>
    </tr>

    <tr>
      <td>
        pt_default_dl
      </td>

      <td>
        Required
      </td>

      <td>
        Default Deep Link for Push Notification
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link for first/all star(s)
      </td>
    </tr>

    <tr>
      <td>
        pt_bg
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_bg_dark
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_dl2
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link for second star
      </td>
    </tr>

    <tr>
      <td>
        pt_dl3
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link for third star
      </td>
    </tr>

    <tr>
      <td>
        pt_dl4
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link for fourth star
      </td>
    </tr>

    <tr>
      <td>
        pt_dl5
      </td>

      <td>
        Optional
      </td>

      <td>
        Deep Link for fifth star
      </td>
    </tr>

    <tr>
      <td>
        pt_ico
      </td>

      <td>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td>
        wzrk_hide_large_icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications  
        Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:push-templates-android#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img
      </td>

      <td>
        Optional
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image
      </td>
    </tr>

    <tr>
      <td>
        pt_gif
      </td>

      <td>
        Optional
      </td>

      <td>
        GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_gif_frames
      </td>

      <td>
        Optional
      </td>

      <td>
        Number of frames to extract from the GIF
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
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

<Image align="center" alt="Product Catalog Template- Vertical View" border={true} caption="Product Catalog Template- Vertical View" src="https://files.readme.io/21393452affbcdbd00b8a7550c07b59db3a855a46f0746099529338ef5208a2f-image.png" width="35% " />

## Linear View

Use the following keys to enable the linear view variant of this template:

|              Template Key | Required | Value  |
| ------------------------: | :------: | :----- |
| pt_product_display_linear | Optional | `true` |

<Image align="center" alt="Product Catalog Template-Linear View" border={true} caption="Product Catalog Template-Linear View" src="https://files.readme.io/1e2e81891b1ce9faa0a12731f254d616dcf51818c28f14f6428c0a5d833855b8-image.png" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Product Catalog Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Value - `pt_product_display`
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Message
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Title
      </td>
    </tr>

    <tr>
      <td>
        pt_img1
      </td>

      <td>
        Required
      </td>

      <td>
        Image One
      </td>
    </tr>

    <tr>
      <td>
        pt_img1_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image One
      </td>
    </tr>

    <tr>
      <td>
        pt_img2
      </td>

      <td>
        Required
      </td>

      <td>
        Image Two
      </td>
    </tr>

    <tr>
      <td>
        pt_img2_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image Two
      </td>
    </tr>

    <tr>
      <td>
        pt_img3
      </td>

      <td>
        Required
      </td>

      <td>
        Image Three
      </td>
    </tr>

    <tr>
      <td>
        pt_img3_alt_text
      </td>

      <td>
        Optional
      </td>

      <td>
        Alt Text for Image Three
      </td>
    </tr>

    <tr>
      <td>
        pt_bt1
      </td>

      <td>
        Required
      </td>

      <td>
        Big text for first image
      </td>
    </tr>

    <tr>
      <td>
        pt_bt2
      </td>

      <td>
        Required
      </td>

      <td>
        Big text for second image
      </td>
    </tr>

    <tr>
      <td>
        pt_bt3
      </td>

      <td>
        Required
      </td>

      <td>
        Big text for third image
      </td>
    </tr>

    <tr>
      <td>
        pt_st2
      </td>

      <td>
        Required
      </td>

      <td>
        Small text for second image
      </td>
    </tr>

    <tr>
      <td>
        pt_st1
      </td>

      <td>
        Required
      </td>

      <td>
        Small text for first image
      </td>
    </tr>

    <tr>
      <td>
        pt_st3
      </td>

      <td>
        Required
      </td>

      <td>
        Small text for third image
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link for first image
      </td>
    </tr>

    <tr>
      <td>
        pt_dl2
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link for second image
      </td>
    </tr>

    <tr>
      <td>
        pt_dl3
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link for third image
      </td>
    </tr>

    <tr>
      <td>
        pt_price1
      </td>

      <td>
        Required
      </td>

      <td>
        Price for first image
      </td>
    </tr>

    <tr>
      <td>
        pt_price2
      </td>

      <td>
        Required
      </td>

      <td>
        Price for second image
      </td>
    </tr>

    <tr>
      <td>
        pt_price3
      </td>

      <td>
        Required
      </td>

      <td>
        Price for third image
      </td>
    </tr>

    <tr>
      <td>
        pt_bg
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_bg_dark
      </td>

      <td>
        Required
      </td>

      <td>
        Background Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_product_display_action
      </td>

      <td>
        Required
      </td>

      <td>
        Action Button Label Text
      </td>
    </tr>

    <tr>
      <td>
        pt_product_display_action_clr
      </td>

      <td>
        Required
      </td>

      <td>
        Action Button Background Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_product_display_linear
      </td>

      <td>
        Optional
      </td>

      <td>
        Linear Layout Template ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_sticky
      </td>

      <td>
        Optional
      </td>

      <td>
        Should the notification be sticky? ("true"/"false")
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto dismiss the notification after a set time (value in seconds)
      </td>
    </tr>

    <tr>
      <td>
        pt_scale_type
      </td>

      <td>
        Optional
      </td>

      <td>
        Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  
        For more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Input Box Template

The Input Box Template lets you collect any input, including feedback from your users. It has four variants.

## With CTAs

The CTA variant of the Input Box Template uses action buttons on the notification to collect input from the user.

To set the CTAs, use the Advanced Options when setting up the campaign on the dashboard.

<Image align="center" alt={618} border={true} caption="Input Box Template-With CTAs" title="Input Box Template-With CTAs" src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputctabasicdismiss.gif" width="35% " />

|        Template Key | Required | Value                                                        |
| ------------------: | :------: | :----------------------------------------------------------- |
| pt_dismiss_on_click | Optional | `true` (Dismisses the notification without opening the app). |

> 📘 **Display the Notification in the tray (for Android 12 and above)**
>
> To keep the notification in the notification tray even after clicking the CTA, set the value for the `pt_dismiss_on_click` payload key as `false` and add the `dismissNotification` function to your code.

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

|                         Template Key | Required | Value                                                                                                                                                                  |
| -----------------------------------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                        pt_event_name | Required | For example: `Remind Later`,                                                                                                                                           |
| pt_event_property_\<property_name_1> | Optional | For example: `<property_value>`,                                                                                                                                       |
| pt_event_property_\<property_name_2> | Optional | future epoch timestamp. For example: `\$D_1592503813`                                                                                                                  |
|                  pt_dismiss_on_click | Required | Value should be `true`. It dismisses the notification without opening the app and raises a required event to the user profile, needed to send a reminder notification. |

<Image align="center" alt={618} border={true} caption="Input Box Template-CTAs with Remind Later option" title="Input Box Template-CTAs with Remind Later option" src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaRemind.gif" width="35% " />

## Reply as an Event

This variant raises an event capturing the user's input as an event property. The app is not opened after the user sends the reply.

To use this variant, use the following values for the keys.

|                         Template Key | Required | Value                                   |
| -----------------------------------: | :------: | :-------------------------------------- |
|                       pt_input_label | Required | for example, `Search`                   |
|                    pt_input_feedback | Required | for example, `Thanks for your feedback` |
|                        pt_event_name | Required | for example. `Searched`,                |
| pt_event_property_\<property_name_2> | Required | fixed value - `pt_input_reply`          |
| pt_event_property_\<property_name_1> | Optional | for example. `<property_value>`,        |

<Image align="center" alt={618} border={true} caption="Input Box Template-Reply as an Event" title="Input Box Template-Reply as an Event" src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaNoOpen.gif" width="35% " />

## Reply as an Intent

This variant passes the reply to the app as an Intent. The app can then process the reply and take appropriate actions.

To use this variant, use the following values for the keys.

|       Template Key | Required | Value                                    |
| -----------------: | :------: | :--------------------------------------- |
|     pt_input_label | Required | for example, `Search`                    |
|  pt_input_feedback | Required | for example., `Thanks for your feedback` |
| pt_input_auto_open | Required | fixed value - `true`                     |

To capture the input, the app can get the `pt_input_reply` key from the Intent extras.

<Image align="center" alt={618} border={true} caption="Input Box Template-Reply as an Intent" title="Input Box Template-Reply as an Intent" src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaWithOpen.gif" width="35% " />

## Template Keys

<Table align={["right","center","left"]}>
  <thead>
    <tr>
      <th>
        Input Box Template Keys
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        pt_id
      </td>

      <td>
        Required
      </td>

      <td>
        Value - `pt_input`
      </td>
    </tr>

    <tr>
      <td>
        pt_title
      </td>

      <td>
        Required
      </td>

      <td>
        Title
      </td>
    </tr>

    <tr>
      <td>
        pt_msg
      </td>

      <td>
        Required
      </td>

      <td>
        Message
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img
      </td>

      <td>
        Required
      </td>

      <td>
        Image
      </td>
    </tr>

    <tr>
      <td>
        pt_big_img_alt
      </td>

      <td>
        Optional
      </td>

      <td>
        Image to be shown after feedback is collected
      </td>
    </tr>

    <tr>
      <td>
        pt_input_label
      </td>

      <td>
        Required
      </td>

      <td>
        Label text to be shown on the input
      </td>
    </tr>

    <tr>
      <td>
        pt_input_feedback
      </td>

      <td>
        Required
      </td>

      <td>
        Feedback
      </td>
    </tr>

    <tr>
      <td>
        pt_dl1
      </td>

      <td>
        Required
      </td>

      <td>
        Deep Link
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_summary
      </td>

      <td>
        Optional
      </td>

      <td>
        Message line when Notification is expanded
      </td>
    </tr>

    <tr>
      <td>
        pt_subtitle
      </td>

      <td>
        Optional
      </td>

      <td>
        Subtitle
      </td>
    </tr>

    <tr>
      <td>
        pt_event_name
      </td>

      <td>
        Optional
      </td>

      <td>
        Name of Event to be raised
      </td>
    </tr>

    <tr>
      <td>
        pt_event_property_\<property_name_1>
      </td>

      <td>
        Optional
      </td>

      <td>
        Value for event property \<property_name_1>
      </td>
    </tr>

    <tr>
      <td>
        pt_event_property_\<property_name_2>
      </td>

      <td>
        Optional
      </td>

      <td>
        Value for event property \<property_name_2>
      </td>
    </tr>

    <tr>
      <td>
        pt_event_property_\<property_name_n>
      </td>

      <td>
        Optional
      </td>

      <td>
        Value for event property \<property_name_n>
      </td>
    </tr>

    <tr>
      <td>
        pt_input_auto_open
      </td>

      <td>
        Optional
      </td>

      <td>
        Auto open the app after feedback
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_title_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Title Color in HEX in dark mode.
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_msg_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Message Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX
      </td>
    </tr>

    <tr>
      <td>
        pt_small_icon_clr_dark
      </td>

      <td>
        Optional
      </td>

      <td>
        Small Icon Color in HEX in dark mode
      </td>
    </tr>

    <tr>
      <td>
        pt_ico
      </td>

      <td>
        Optional
      </td>

      <td>
        Large Icon
      </td>
    </tr>

    <tr>
      <td>
        wzrk_hide_large_icon
      </td>

      <td>
        Optional
      </td>

      <td>
        Hide the large icon in Android notifications  
        Values:

        <li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>
      </td>
    </tr>

    <tr>
      <td>
        pt_dismiss_on_click
      </td>

      <td>
        Optional
      </td>

      <td>
        Dismiss notification on click
      </td>
    </tr>

    <tr>
      <td>
        pt_json
      </td>

      <td>
        Optional
      </td>

      <td>
        Above keys in JSON format
      </td>
    </tr>
  </tbody>
</Table>

# Template Variations

Users can preview and render different variations of outputs for each Push template type based on the following factors:

* **Version-based Output**: Notifications render differently for Android version 12 and above and version 11 and below. Key considerations include the following:
  * **Android 13 and above**: Use images no larger than 450x450 px for optimal rendering.
  * **Android 12 and above**: Center-align any text within images is centered for to ensure better visibility.
  * **Android 11 and 12**: Use image file size below 100 KB to prevent rendering issues.
  * **Android 11 and below**: Use images of under 3 MB or lower is recommended for better to maintain optimal performance.
* **CTA-based Output**: Notification renders based on whether CTA buttons are included in the payload.

# Dark Mode

Push templates automatically adapt to the device's theme settings, rendering notifications in dark or light mode.

| **Theme Setting**      | **Notification Appearance**                       | Sample Output                                                                                           |
| ---------------------- | ------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| **Dark mode enabled**  | Notification renders with a **dark background**   | ![](https://files.readme.io/37884bc08c5375da82457ffc4f54c7b31d3dc1591e0430f65949b73003df5e35-image.png) |
| **Dark mode disabled** | Notification renders with a **light background**. | ![](https://files.readme.io/e5e9256facd596cdea7b325ad9945868ed3a4d3c5527ef09d46ae64ae350fc5d-image.png) |

> 📘 **Custom Color Definitions**
>
> Templates support **custom color definitions** that adapt to both themes for visual consistency.

> 🚧 **SDK Version Requirement**
>
> To use the Dark mode feature, ensure you are using Push Templates SDK version 2.1.0 and above.

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

> 🚧 **SDK Version Requirement**
>
> To use the Image scaling feature, ensure you are using Push Templates SDK version 2.1.0 and above.

# Image Specifications

The following are the image specifications and guidelines for the Push Templates:

<Table align={["right","left","left"]}>
  <thead>
    <tr>
      <th>
        Template
      </th>

      <th>
        Aspect Ratios (Approx.)
      </th>

      <th>
        Maximum File Size

        (OS version 12 and above)
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **Standard**
      </td>

      <td>
        <li> OS version 12 and above: **3:2**</li> <li>OS version 11 and below: **5:3**</li>
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        **Advanced**
      </td>

      <td>
        <li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>
      </td>

      <td>
        500 KB
      </td>
    </tr>

    <tr>
      <td>
        **Auto Carousel**
      </td>

      <td>
        <li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>
      </td>

      <td>
        86 KB
      </td>
    </tr>

    <tr>
      <td>
        **Manual Carousel**
      </td>

      <td>
        <li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>
      </td>

      <td>
        86 KB
      </td>
    </tr>

    <tr>
      <td>
        **Five Icon**
      </td>

      <td>
        <li> OS version 12 and above: **1:1**</li><li>OS version 11 and below: **1:1**</li>
      </td>

      <td>
        50 KB
      </td>
    </tr>

    <tr>
      <td>
        **Text over Image**
      </td>

      <td>
        <li>OS version 12 and above: **1:1**</li><li>OS version 11 and below: **2:1**</li>
      </td>

      <td>
        500 KB
      </td>
    </tr>

    <tr>
      <td>
        **Timer**
      </td>

      <td>
        <li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>
      </td>

      <td>
        326 KB
      </td>
    </tr>

    <tr>
      <td>
        **Product Catalogue Linear View**
      </td>

      <td>
        <li>OS version 12 and above - **3:2**</li><li>OS version 11 and below- **2:1**</li>
      </td>

      <td>
        86 KB
      </td>
    </tr>

    <tr>
      <td>
        **Product Catalogue Vertical View**
      </td>

      <td>
        <li>OS version 12 and above - **3:4**</li><li>OS version 11 and below- **1:1**</li>
      </td>

      <td>
        86 KB
      </td>
    </tr>

    <tr>
      <td>
        **Input Box**
      </td>

      <td>
        <li>OS version 12 and above - **3:2**</li><li>OS version 11 and below- **5:3**</li>
      </td>

      <td>
        500 KB
      </td>
    </tr>

    <tr>
      <td>
        **Ratings**
      </td>

      <td>
        <li>OS version 12 and above - **3:1**</li><li>OS version 11 and below- **5:3**</li>
      </td>

      <td>
        500 KB
      </td>
    </tr>
  </tbody>
</Table>

# Image Guidelines

Ensure images for the following templates meet the specified size guidelines:

| Template Name            | Recommended Resolution |
| :----------------------- | :--------------------- |
| Auto Carousel Template   | 400 x 300 px           |
| Manual Carousel Template | 240 x 180 px           |
| Five Icon  Template      | 300 x 300 px           |

For Text over Image Template, ensure the text is center-aligned within the image for devices running OS version 12 and above.

# GIF Size Guidelines

To use GIFs in the Push Templates, use Push Templates SDK version 2.2.0 and above. Also, make sure GIFs meet the specified size guidelines. You can use [Ezgif tool](https://ezgif.com/resize) to resize the GIF.

| GIF Size     | Recommended Number of Frames |
| :----------- | :--------------------------- |
| 180 x 120 px | 15                           |
| 240 x 160 px | 10                           |
| 300 x 200 px | 6                            |

# FAQs

For FAQs, refer to [Push Templates FAQs](https://docs.clevertap.com/docs/troubleshooting-faqs-push-notifications).

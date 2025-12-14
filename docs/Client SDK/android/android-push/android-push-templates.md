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
implementation "com.clevertap.android:clevertap-android-sdk:7.7.1" // 7.4.1 and above
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

# Standard Push Template

Standard Template is a default template for push notifications received on apps that supports a simple layout with title, message, image, and optional buttons, ideal for quick, informative alerts. For example, an e-commerce business might want to inform customers that their order is on the way. To use a GIF in a standard push template, pass the key `wzrk_gif` with the value set to the URL of the GIF image.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7a4e66924248b609d31ccdc200b05788c5a2b1b5e6550e58de559233caefe4b2-image.png",
        null,
        "Standard Push Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "caption": "Standard Push Template"
    }
  ]
}
[/block]


## GIF Support in Standard Push Template

CleverTap supports GIFs in standard push templates from SDK version 7.5.0 on devices running Android 14 and above. This allows you to add animated media to push notifications, enhancing user engagement through dynamic visuals.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/47c076dffe3d89c693a3fc4aeac5117649dc0a28af5e2b820157f86d7919ed29-Screen_Recording_Jul_28_2025_1.gif",
        "",
        ""
      ],
      "align": "center",
      "sizing": "35% "
    }
  ]
}
[/block]


### Specifications and Design Guidelines

- Supported on Android 14+ with SDK v7.5.0 or later.
- Applies only to the Standard push template.
- For older Android versions or SDKs, CleverTap shows the first frame of the GIF as a static image.
- Use the recommended aspect ratio for the selected template to avoid cropping or distortion.
- Keep file size under 1MB for smooth rendering.
- Opt for looped, visually clear GIFs that work well even if partially cropped.

### Limitations

- Devices running Android 13 and below do not support GIF playback; a static fallback image is shown.
- Some OEMs may not fully support GIF rendering, even on Android 14 and above.
- On certain devices, the heads-up notification may display only the first frame, while the full GIF appears when the notification tray is expanded.

# Advanced Push Template

This template supports rich media, deep links, and enhanced customization for more engaging messages. It is ideal for interactive, visually appealing messages. For example, businesses can use it to build excitement for new product drops, early access offers, or feature rollouts that benefit from strong visuals and clear Call to Action (CTAs).

For template outputs Android 11 and below, refer to [Push Templates on Android v11 and below](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESANDROID12.md).

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/65cc6bcd2523518ceab5cdc452022fbc4f4bce889e87469c883fd394b1fb18e4-image.png",
        null,
        "Advanced Push Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "caption": "Advanced Push Template"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Advanced Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Indicates the type of template, for example, `pt_basic`.",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.",
    "2-0": "pt_msg",
    "2-1": "Required",
    "2-2": "Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.",
    "3-0": "pt_msg_summary",
    "3-1": "Required",
    "3-2": "Message line when Notification is expanded.",
    "4-0": "pt_subtitle",
    "4-1": "Optional",
    "4-2": "Subtitle",
    "5-0": "pt_bg",
    "5-1": "Required",
    "5-2": "Background color of your push notification. You can use it to match your brand theme or draw visual attention to the notification. It accepts HEX codes or color pickers.",
    "6-0": "pt_bg_dark",
    "6-1": "Optional",
    "6-2": "Background color of your push notification in dark mode",
    "7-0": "pt_big_img",
    "7-1": "Optional",
    "7-2": "Image",
    "8-0": "pt_big_img_alt_text",
    "8-1": "Optional",
    "8-2": "Alt Text for Image",
    "9-0": "pt_gif",
    "9-1": "Optional",
    "9-2": "GIF",
    "10-0": "pt_gif_frames",
    "10-1": "Optional",
    "10-2": "Number of frames to extract from the GIF",
    "11-0": "pt_ico",
    "11-1": "Optional",
    "11-2": "Large Icon",
    "12-0": "wzrk_hide_large_icon",
    "12-1": "Optional",
    "12-2": "Hide the large icon in Android notifications  \nValues:  \n  \n<li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>",
    "13-0": "pt_dl1",
    "13-1": "Optional",
    "13-2": "One Deep Link (minimum)",
    "14-0": "pt_title_clr",
    "14-1": "Optional",
    "14-2": "Title Color in HEX",
    "15-0": "pt_title_clr_dark",
    "15-1": "Optional",
    "15-2": "Title Color in HEX in dark mode",
    "16-0": "pt_msg_clr",
    "16-1": "Optional",
    "16-2": "Message Color in HEX",
    "17-0": "pt_msg_clr_dark",
    "17-1": "Optional",
    "17-2": "Message Color in HEX in dark mode",
    "18-0": "pt_small_icon_clr",
    "18-1": "Optional",
    "18-2": "Small Icon Color in HEX",
    "19-0": "pt_small_icon_clr_dark",
    "19-1": "Optional",
    "19-2": "Small Icon Color in HEX in dark mode",
    "20-0": "pt_scale_type",
    "20-1": "Optional",
    "20-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).",
    "21-0": "pt_sticky",
    "21-1": "Optional",
    "21-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "22-0": "pt_dismiss",
    "22-1": "Optional",
    "22-2": "Auto dismiss the notification after a set time (value in seconds)",
    "23-0": "pt_json",
    "23-1": "Optional",
    "23-2": "Above template keys in JSON format"
  },
  "cols": 3,
  "rows": 24,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Auto Carousel Template

Auto Carousel is an automatic revolving carousel push notification that automatically cycles through multiple cards or slides within a single notification. Each card typically includes an image, title, description, and a CTA button, allowing users to view multiple offers, messages, or product highlights without leaving their device’s notification tray.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b06f01f282ddb61ab6bca44b51c79e65c9735b224432fadaf0799eb6c1dd7a70-Screen_Recording_Jun_4_2025_from_Google_Drive.mp4_1.gif",
        null,
        "Auto Carousel Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "caption": "Auto Carousel Template"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Auto Carousel Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Indicates the type of template, for example, `pt_carousel`.",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.",
    "2-0": "pt_msg",
    "2-1": "Required",
    "2-2": "Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.",
    "3-0": "pt_msg_summary",
    "3-1": "Optional",
    "3-2": "Message line when Notification is expanded.",
    "4-0": "pt_subtitle",
    "4-1": "Optional",
    "4-2": "Subtitle",
    "5-0": "pt_dl1",
    "5-1": "Required",
    "5-2": "Deep Link (Max one)",
    "6-0": "pt_img1",
    "6-1": "Required",
    "6-2": "Image One",
    "7-0": "pt_img1_alt_text",
    "7-1": "Optional",
    "7-2": "Alt Text for Image One",
    "8-0": "pt_img2",
    "8-1": "Required",
    "8-2": "Image Two",
    "9-0": "pt_img2_alt_text",
    "9-1": "Optional",
    "9-2": "Alt Text for Image Two",
    "10-0": "pt_img3",
    "10-1": "Required",
    "10-2": "Image Three",
    "11-0": "pt_img3_alt_text",
    "11-1": "Optional",
    "11-2": "Alt Text for Image Three",
    "12-0": "pt_img`n`",
    "12-1": "Optional",
    "12-2": "Image `N`",
    "13-0": "pt_img`n`\\_alt_text",
    "13-1": "Optional",
    "13-2": "Alt Text for Image `N`",
    "14-0": "pt_bg",
    "14-1": "Required",
    "14-2": "Background Color in HEX",
    "15-0": "pt_bg_dark",
    "15-1": "Required",
    "15-2": "Background Color in HEX in dark mode",
    "16-0": "pt_ico",
    "16-1": "Optional",
    "16-2": "Large Icon",
    "17-0": "wzrk_hide_large_icon",
    "17-1": "Optional",
    "17-2": "Hide the large icon in Android notifications  \nValues:  \n  \n<li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>",
    "18-0": "pt_title_clr",
    "18-1": "Optional",
    "18-2": "Title Color in HEX",
    "19-0": "pt_title_clr_dark",
    "19-1": "Optional",
    "19-2": "Title Color in HEX in dark mode.",
    "20-0": "pt_msg_clr",
    "20-1": "Optional",
    "20-2": "Message Color in HEX",
    "21-0": "pt_msg_clr_dark",
    "21-1": "Optional",
    "21-2": "Message Color in HEX in dark mode",
    "22-0": "pt_small_icon_clr",
    "22-1": "Optional",
    "22-2": "Small Icon Color in HEX",
    "23-0": "pt_small_icon_clr_dark",
    "23-1": "Optional",
    "23-2": "Small Icon Color in HEX in dark mode",
    "24-0": "pt_sticky",
    "24-1": "Optional",
    "24-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "25-0": "pt_dismiss",
    "25-1": "Optional",
    "25-2": "Auto dismiss the notification after a set time (value in seconds)",
    "26-0": "pt_scale_type",
    "26-1": "Optional",
    "26-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).",
    "27-0": "pt_json",
    "27-1": "Optional",
    "27-2": "Above keys in JSON format"
  },
  "cols": 3,
  "rows": 28,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Manual Carousel Template

Manual Carousel is an interactive push notification format that allows users to browse through multiple cards or slides at their own pace. Each card typically features rich content such as images, titles, short descriptions, and a CTA. Navigation is user-controlled, with arrows or swipe gestures enabling the transition between cards.

If only a single image is available for download, this template falls back to the Basic Template.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/314a341e4efcff60b16c13c2d9d89c91866ff0a66eb39458de68670c9d1d7ab7-Screen_Recording_Jun_4_2025_from_Google_Drive_1.mp4_1.gif",
        null,
        "Manual Carousel Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Manual Carousel Template"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Manual Carousel Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Indicates the type of template, for example,`pt_manual_carousel`",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.",
    "2-0": "pt_msg",
    "2-1": "Required",
    "2-2": "Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.",
    "3-0": "pt_msg_summary",
    "3-1": "Optional",
    "3-2": "Message line when Notification is expanded",
    "4-0": "pt_subtitle",
    "4-1": "Optional",
    "4-2": "Subtitle",
    "5-0": "pt_dl1",
    "5-1": "Required",
    "5-2": "Deep Link One",
    "6-0": "pt_dl2",
    "6-1": "Optional",
    "6-2": "Deep Link Two",
    "7-0": "pt_dl`n`",
    "7-1": "Optional",
    "7-2": "Deep Link for the nth image",
    "8-0": "pt_img1",
    "8-1": "Required",
    "8-2": "Image One",
    "9-0": "pt_img1_alt_text",
    "9-1": "Optional",
    "9-2": "Alt Text for Image One",
    "10-0": "pt_img2",
    "10-1": "Required",
    "10-2": "Image Two",
    "11-0": "pt_img2_alt_text",
    "11-1": "Optional",
    "11-2": "Alt Text for Image Two",
    "12-0": "pt_img3",
    "12-1": "Required",
    "12-2": "Image Three",
    "13-0": "pt_img3_alt_text",
    "13-1": "Optional",
    "13-2": "Alt Text for Image Three",
    "14-0": "pt_img`n`",
    "14-1": "Optional",
    "14-2": "Image `N`",
    "15-0": "pt_img`n`\\_alt_text",
    "15-1": "Optional",
    "15-2": "Alt Text for Image `N`",
    "16-0": "pt_bg",
    "16-1": "Required",
    "16-2": "Background Color in HEX",
    "17-0": "pt_bg_dark",
    "17-1": "Required",
    "17-2": "Background Color in HEX in dark mode",
    "18-0": "pt_ico",
    "18-1": "Optional",
    "18-2": "Large Icon",
    "19-0": "wzrk_hide_large_icon",
    "19-1": "Optional",
    "19-2": "Hide the large icon in Android notifications  \nValues:  \n  \n<li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>",
    "20-0": "pt_title_clr",
    "20-1": "Optional",
    "20-2": "Title Color in HEX",
    "21-0": "pt_title_clr_dark",
    "21-1": "Optional",
    "21-2": "Title Color in HEX in dark mode",
    "22-0": "pt_msg_clr",
    "22-1": "Optional",
    "22-2": "Message Color in HEX",
    "23-0": "pt_msg_clr_dark",
    "23-1": "Optional",
    "23-2": "Message Color in HEX in dark mode",
    "24-0": "pt_small_icon_clr",
    "24-1": "Optional",
    "24-2": "Small Icon Color in HEX",
    "25-0": "pt_small_icon_clr_dark",
    "25-1": "Optional",
    "25-2": "Small Icon Color in HEX in dark mode",
    "26-0": "pt_sticky",
    "26-1": "Optional",
    "26-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "27-0": "pt_dismiss",
    "27-1": "Optional",
    "27-2": "Auto dismiss the notification after a set time (value in seconds)",
    "28-0": "pt_scale_type",
    "28-1": "Optional",
    "28-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).",
    "29-0": "pt_json",
    "29-1": "Optional",
    "29-2": "Above keys in JSON format",
    "30-0": "pt_manual_carousel_type",
    "30-1": "Optional",
    "30-2": "`filmstrip`",
    "31-0": "pt_gif_frames",
    "31-1": "Optional",
    "31-2": "Number of frames to extract from the GIF"
  },
  "cols": 3,
  "rows": 32,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Five Icons Template

The Five Icons template is a sticky push notification with no text, just five icons, and a close button that can help your users go directly to the functionality of their choice with a button click.

The library does not notify if at least three icons are not retrieved. The bifurcation of each CTA is captured in the event Notification Clicked within the property `wzrk_c2a`.

By default, if the user clicks on any notification area except the five and close icons, an activity intent launches.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/563c039e56f135aea167f9fecda45c428087ada312e0013d1347567fcd8f724e-image.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "50% ",
      "border": true
    }
  ]
}
[/block]


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

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a0dd4a50b5b7783a1f970928b1d13d16d660728b9afb935e4947fbd3c942fb00-Timer_12_Light_Live.mp4_1.gif",
        null,
        "Timer Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "caption": "Timer Template"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Timer Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Indicates the type of template, for example, `pt_timer`",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.",
    "2-0": "pt_title_alt",
    "2-1": "Optional",
    "2-2": "Title to show after timer expires",
    "3-0": "pt_msg",
    "3-1": "Required",
    "3-2": "Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.",
    "4-0": "pt_msg_alt",
    "4-1": "Optional",
    "4-2": "Message to show after timer expires",
    "5-0": "pt_msg_summary",
    "5-1": "Optional",
    "5-2": "Message line when Notification is expanded",
    "6-0": "pt_subtitle",
    "6-1": "Optional",
    "6-2": "Subtitle",
    "7-0": "pt_dl1",
    "7-1": "Required",
    "7-2": "Deep Link",
    "8-0": "pt_big_img",
    "8-1": "Optional",
    "8-2": "Image",
    "9-0": "pt_big_img_alt",
    "9-1": "Optional",
    "9-2": "Image to show when timer expires",
    "10-0": "   pt_big_img_alt_text",
    "10-1": "Optional",
    "10-2": "Alt Text for Image",
    "11-0": "pt_big_img_alt_alt_text",
    "11-1": "Optional",
    "11-2": "Alt Text for Image to show when timer expires",
    "12-0": "pt_gif",
    "12-1": "Optional",
    "12-2": "GIF",
    "13-0": "pt_gif_frames",
    "13-1": "Optional",
    "13-2": "Number of frames to extract from the GIF",
    "14-0": "pt_bg",
    "14-1": "Required",
    "14-2": "Background Color in HEX",
    "15-0": "pt_bg_dark",
    "15-1": "Required",
    "15-2": "Background Color in HEX in dark mode",
    "16-0": "pt_chrono_title_clr",
    "16-1": "Optional",
    "16-2": "Color for timer text in HEX",
    "17-0": "pt_chrono_title_clr_dark",
    "17-1": "Optional",
    "17-2": "Color for timer text in HEX in dark mode",
    "18-0": "pt_timer_threshold",
    "18-1": "Required",
    "18-2": "Timer duration in seconds (minimum 10)",
    "19-0": "pt_timer_end",
    "19-1": "Optional",
    "19-2": "Epoch Timestamp to countdown to (for example, $D_1595871380 or 1595871380). Not required if `pt_timer_threshold` is specified.",
    "20-0": "pt_title_clr",
    "20-1": "Optional",
    "20-2": "Title Color in HEX",
    "21-0": "pt_title_clr_dark",
    "21-1": "Optional",
    "21-2": "Title Color in HEX in dark mode",
    "22-0": "pt_msg_clr",
    "22-1": "Optional",
    "22-2": "Message Color in HEX",
    "23-0": "pt_msg_clr_dark",
    "23-1": "Optional",
    "23-2": "Message Color in HEX in dark mode",
    "24-0": "pt_small_icon_clr",
    "24-1": "Optional",
    "24-2": "Small Icon Color in HEX",
    "25-0": "pt_small_icon_clr_dark",
    "25-1": "Optional",
    "25-2": "Small Icon Color in HEX in dark mode",
    "26-0": "pt_scale_type",
    "26-1": "Optional",
    "26-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).",
    "27-0": "pt_render_terminal",
    "27-1": "Optional",
    "27-2": "Determines whether terminal notification is rendered  \nValue- `true` or `false`",
    "28-0": "pt_sticky",
    "28-1": "Optional",
    "28-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "29-0": "pt_dismiss",
    "29-1": "Optional",
    "29-2": "Auto dismiss the notification after a set time (value in seconds)",
    "30-0": "pt_msg_summary_alt",
    "30-1": "Optional",
    "30-2": " Summary to display once the timer expires",
    "31-0": "pt_json",
    "31-1": "Optional",
    "31-2": "Above keys in JSON format"
  },
  "cols": 3,
  "rows": 32,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Text over Image Template

Text over Image template is a visually immersive push notification format designed to maximize the use of imagery. In this layout, the background image spans the entire surface area of the notification, creating an edge-to-edge visual experience. All accompanying text such as the title, message, and call-to-action is seamlessly overlaid on top of the image, ensuring the message is prominent while maintaining a clean, modern aesthetic.

The library falls back to the Advanced Template if the image cannot be downloaded.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/aca5987a4cff4a252167cf9c0c954a422b08bfd7cfa7860b6ebb2c7bd0bc40f9-image.png",
        null,
        "Text over Image Push Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "caption": "Text over Image Push Template"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Text over Image Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Indicates the type of template, for example, `pt_zero_bezel`.",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Maps to the campaign title, the main headline of your notification must be concise and attention-grabbing, defining the exact purpose of your campaign.",
    "2-0": "pt_msg",
    "2-1": "Required",
    "2-2": "Maps to the campaign message, the body text of your notification. The message should clearly convey the purpose of your campaign.",
    "3-0": "pt_msg_summary",
    "3-1": "Optional",
    "3-2": "Message line when Notification is expanded.",
    "4-0": "pt_subtitle",
    "4-1": "Optional",
    "4-2": "Subtitle",
    "5-0": "pt_big_img",
    "5-1": "Required",
    "5-2": "Image",
    "6-0": "pt_big_img_alt_text",
    "6-1": "Optional",
    "6-2": "Alt Text for Image",
    "7-0": "pt_gif",
    "7-1": "Optional",
    "7-2": "GIF",
    "8-0": "pt_gif_frames",
    "8-1": "Optional",
    "8-2": "Number of frames to extract from the GIF",
    "9-0": "pt_small_view",
    "9-1": "Optional",
    "9-2": "Select text-only small view layout (`text_only`)",
    "10-0": "pt_dl1",
    "10-1": "Optional",
    "10-2": "Deep Link",
    "11-0": "pt_title_clr",
    "11-1": "Optional",
    "11-2": "Title Color in HEX",
    "12-0": "pt_title_clr_dark",
    "12-1": "Optional",
    "12-2": "Title Color in HEX in dark mode",
    "13-0": "pt_msg_clr",
    "13-1": "Optional",
    "13-2": "Message Color in HEX",
    "14-0": "pt_msg_clr_dark",
    "14-1": "Optional",
    "14-2": "Message Color in HEX in dark mode",
    "15-0": "pt_small_icon_clr",
    "15-1": "Optional",
    "15-2": "Small Icon Color in HEX",
    "16-0": "pt_small_icon_clr_dark",
    "16-1": "Optional",
    "16-2": "Small Icon Color in HEX",
    "17-0": "pt_scale_type",
    "17-1": "Optional",
    "17-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).",
    "18-0": "pt_big_img_collapsed",
    "18-1": "Optional",
    "18-2": "Image for the collapsed view",
    "19-0": "pt_gif_collapsed",
    "19-1": "Optional",
    "19-2": "GIF for the collapsed view",
    "20-0": "pt_gif_frames_collapsed",
    "20-1": "Optional",
    "20-2": "Number of frames to extract from the GIF for the collapsed view",
    "21-0": "pt_big_img_collapsed_alt_text",
    "21-1": "Optional",
    "21-2": "Alt Text for Image in the collapsed view",
    "22-0": "pt_scale_type_collapsed",
    "22-1": "Optional",
    "22-2": "ScaleType for the image in the collapsed View (\"center_crop\"/\"fit_center\")",
    "23-0": "pt_sticky",
    "23-1": "Optional",
    "23-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "24-0": "pt_dismiss",
    "24-1": "Optional",
    "24-2": "Auto dismiss the notification after a set time (value in seconds)",
    "25-0": "pt_ico",
    "25-1": "Optional",
    "25-2": "Large Icon",
    "26-0": "wzrk_hide_large_icon",
    "26-1": "Optional",
    "26-2": "Hide the large icon in Android notifications  \nValues:  \n  \n<li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>",
    "27-0": "pt_json",
    "27-1": "Optional",
    "27-2": "Above keys in JSON format"
  },
  "cols": 3,
  "rows": 28,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Rating Template

Rating template lets your users give you feedback. This feedback is captured in the event _Rating Submitted_ within the property `wzrk_c2a`.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e026a6e56be3574972ff15ae648e46be20699f55cd77eac60f318c3d77c3948e-image.png",
        null,
        "Rating Template"
      ],
      "align": "center",
      "sizing": "35% ",
      "caption": "Rating Template"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Rating Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Value - `pt_rating`",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Title",
    "2-0": "pt_msg",
    "2-1": "Required",
    "2-2": "Message",
    "3-0": "pt_default_dl",
    "3-1": "Required",
    "3-2": "Default Deep Link for Push Notification",
    "4-0": "pt_dl1",
    "4-1": "Required",
    "4-2": "Deep Link for first/all star(s)",
    "5-0": "pt_bg",
    "5-1": "Required",
    "5-2": "Background Color in HEX",
    "6-0": "pt_bg_dark",
    "6-1": "Required",
    "6-2": "Background Color in HEX in dark mode",
    "7-0": "pt_msg_summary",
    "7-1": "Optional",
    "7-2": "Message line when Notification is expanded",
    "8-0": "pt_subtitle",
    "8-1": "Optional",
    "8-2": "Subtitle",
    "9-0": "pt_dl2",
    "9-1": "Optional",
    "9-2": "Deep Link for second star",
    "10-0": "pt_dl3",
    "10-1": "Optional",
    "10-2": "Deep Link for third star",
    "11-0": "pt_dl4",
    "11-1": "Optional",
    "11-2": "Deep Link for fourth star",
    "12-0": "pt_dl5",
    "12-1": "Optional",
    "12-2": "Deep Link for fifth star",
    "13-0": "pt_ico",
    "13-1": "Optional",
    "13-2": "Large Icon",
    "14-0": "wzrk_hide_large_icon",
    "14-1": "Optional",
    "14-2": "Hide the large icon in Android notifications  \nValues:  \n  \n<li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>",
    "15-0": "pt_title_clr",
    "15-1": "Optional",
    "15-2": "Title Color in HEX",
    "16-0": "pt_title_clr_dark",
    "16-1": "Optional",
    "16-2": "Title Color in HEX in dark mode.",
    "17-0": "pt_msg_clr",
    "17-1": "Optional",
    "17-2": "Message Color in HEX",
    "18-0": "pt_msg_clr_dark",
    "18-1": "Optional",
    "18-2": "Message Color in HEX in dark mode",
    "19-0": "pt_small_icon_clr",
    "19-1": "Optional",
    "19-2": "Small Icon Color in HEX",
    "20-0": "pt_small_icon_clr_dark",
    "20-1": "Optional",
    "20-2": "Small Icon Color in HEX in dark mode",
    "21-0": "pt_scale_type",
    "21-1": "Optional",
    "21-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:push-templates-android#image-scaling).",
    "22-0": "pt_json",
    "22-1": "Optional",
    "22-2": "Above keys in JSON format",
    "23-0": "pt_big_img",
    "23-1": "Optional",
    "23-2": "Image",
    "24-0": "pt_big_img_alt_text",
    "24-1": "Optional",
    "24-2": "Alt Text for Image",
    "25-0": "pt_gif",
    "25-1": "Optional",
    "25-2": "GIF",
    "26-0": "pt_gif_frames",
    "26-1": "Optional",
    "26-2": "Number of frames to extract from the GIF",
    "27-0": "pt_sticky",
    "27-1": "Optional",
    "27-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "28-0": "pt_dismiss",
    "28-1": "Optional",
    "28-2": "Auto dismiss the notification after a set time (value in seconds)"
  },
  "cols": 3,
  "rows": 29,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Product Catalog Template

The product catalog template lets you showcase different product images (or a product catalog) before the user can click on the _BUY NOW_ option, which can take them directly to the product via deep links. This template has two variants.

## Vertical View

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/21393452affbcdbd00b8a7550c07b59db3a855a46f0746099529338ef5208a2f-image.png",
        null,
        "Product Catalog Template- Vertical View"
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Product Catalog Template- Vertical View"
    }
  ]
}
[/block]


## Linear View

Use the following keys to enable the linear view variant of this template:

|              Template Key | Required | Value  |
| ------------------------: | :------: | :----- |
| pt_product_display_linear | Optional | `true` |

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1e2e81891b1ce9faa0a12731f254d616dcf51818c28f14f6428c0a5d833855b8-image.png",
        null,
        "Product Catalog Template-Linear View"
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Product Catalog Template-Linear View"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Product Catalog Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Value - `pt_product_display`",
    "1-0": "pt_msg",
    "1-1": "Required",
    "1-2": "Message",
    "2-0": "pt_title",
    "2-1": "Required",
    "2-2": "Title",
    "3-0": "pt_img1",
    "3-1": "Required",
    "3-2": "Image One",
    "4-0": "pt_img1_alt_text",
    "4-1": "Optional",
    "4-2": "Alt Text for Image One",
    "5-0": "pt_img2",
    "5-1": "Required",
    "5-2": "Image Two",
    "6-0": "pt_img2_alt_text",
    "6-1": "Optional",
    "6-2": "Alt Text for Image Two",
    "7-0": "pt_img3",
    "7-1": "Required",
    "7-2": "Image Three",
    "8-0": "pt_img3_alt_text",
    "8-1": "Optional",
    "8-2": "Alt Text for Image Three",
    "9-0": "pt_bt1",
    "9-1": "Required",
    "9-2": "Big text for first image",
    "10-0": "pt_bt2",
    "10-1": "Required",
    "10-2": "Big text for second image",
    "11-0": "pt_bt3",
    "11-1": "Required",
    "11-2": "Big text for third image",
    "12-0": "pt_st2",
    "12-1": "Required",
    "12-2": "Small text for second image",
    "13-0": "pt_st1",
    "13-1": "Required",
    "13-2": "Small text for first image",
    "14-0": "pt_st3",
    "14-1": "Required",
    "14-2": "Small text for third image",
    "15-0": "pt_dl1",
    "15-1": "Required",
    "15-2": "Deep Link for first image",
    "16-0": "pt_dl2",
    "16-1": "Required",
    "16-2": "Deep Link for second image",
    "17-0": "pt_dl3",
    "17-1": "Required",
    "17-2": "Deep Link for third image",
    "18-0": "pt_price1",
    "18-1": "Required",
    "18-2": "Price for first image",
    "19-0": "pt_price2",
    "19-1": "Required",
    "19-2": "Price for second image",
    "20-0": "pt_price3",
    "20-1": "Required",
    "20-2": "Price for third image",
    "21-0": "pt_bg",
    "21-1": "Required",
    "21-2": "Background Color in HEX",
    "22-0": "pt_bg_dark",
    "22-1": "Required",
    "22-2": "Background Color in HEX in dark mode",
    "23-0": "pt_product_display_action",
    "23-1": "Required",
    "23-2": "Action Button Label Text",
    "24-0": "pt_product_display_action_clr",
    "24-1": "Required",
    "24-2": "Action Button Background Color in HEX",
    "25-0": "pt_subtitle",
    "25-1": "Optional",
    "25-2": "Subtitle",
    "26-0": "pt_product_display_linear",
    "26-1": "Optional",
    "26-2": "Linear Layout Template (\"true\"/\"false\")",
    "27-0": "pt_title_clr",
    "27-1": "Optional",
    "27-2": "Title Color in HEX",
    "28-0": "pt_title_clr_dark",
    "28-1": "Optional",
    "28-2": "Title Color in HEX in dark mode.",
    "29-0": "pt_msg_clr",
    "29-1": "Optional",
    "29-2": "Message Color in HEX",
    "30-0": "pt_msg_clr_dark",
    "30-1": "Optional",
    "30-2": "Message Color in HEX in dark mode",
    "31-0": "pt_small_icon_clr",
    "31-1": "Optional",
    "31-2": "Small Icon Color in HEX",
    "32-0": "pt_small_icon_clr_dark",
    "32-1": "Optional",
    "32-2": "Small Icon Color in HEX in dark mode",
    "33-0": "pt_sticky",
    "33-1": "Optional",
    "33-2": "Should the notification be sticky? (\"true\"/\"false\")",
    "34-0": "pt_dismiss",
    "34-1": "Optional",
    "34-2": "Auto dismiss the notification after a set time (value in seconds)",
    "35-0": "pt_scale_type",
    "35-1": "Optional",
    "35-2": "Specify the image scaling type for notifications.  Available values: <li>`fit_center` </li>  <li>`center_crop`</li>  \nFor more information on how it works, refer to [Image Scaling](doc:android-push-templates#image-scaling).",
    "36-0": "pt_json",
    "36-1": "Optional",
    "36-2": "Above keys in JSON format"
  },
  "cols": 3,
  "rows": 37,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


# Input Box Template

The Input Box Template lets you collect any input, including feedback from your users. It has four variants.

## With CTAs

The CTA variant of the Input Box Template uses action buttons on the notification to collect input from the user.

To set the CTAs, use the Advanced Options when setting up the campaign on the dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputctabasicdismiss.gif",
        "Input Box Template-With CTAs",
        618
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Input Box Template-With CTAs"
    }
  ]
}
[/block]


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

|                            Template Key | Required | Value                                                                                                                                                                  |
| --------------------------------------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                           pt_event_name | Required | For example: `Remind Later`,                                                                                                                                           |
| pt\_event\_property\_\<property_name_1> | Optional | For example: `<property_value>`,                                                                                                                                       |
| pt\_event\_property\_\<property_name_2> | Optional | future epoch timestamp. For example: `\$D_1592503813`                                                                                                                  |
|                     pt_dismiss_on_click | Required | Value should be `true`. It dismisses the notification without opening the app and raises a required event to the user profile, needed to send a reminder notification. |

[block:image]
{
  "images": [
    {
      "image": [
        "https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaRemind.gif",
        "Input Box Template-CTAs with Remind Later option",
        618
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Input Box Template-CTAs with Remind Later option"
    }
  ]
}
[/block]


## Reply as an Event

This variant raises an event capturing the user's input as an event property. The app is not opened after the user sends the reply.

To use this variant, use the following values for the keys.

|                            Template Key | Required | Value                                   |
| --------------------------------------: | :------: | :-------------------------------------- |
|                          pt_input_label | Required | for example, `Search`                   |
|                       pt_input_feedback | Required | for example, `Thanks for your feedback` |
|                          pt\_event_name | Required | for example. `Searched`,                |
| pt\_event\_property\_\<property_name_2> | Required | fixed value - `pt_input_reply`          |
| pt\_event\_property\_\<property_name_1> | Optional | for example. `<property_value>`,        |

[block:image]
{
  "images": [
    {
      "image": [
        "https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaNoOpen.gif",
        "Input Box Template-Reply as an Event",
        618
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Input Box Template-Reply as an Event"
    }
  ]
}
[/block]


## Reply as an Intent

This variant passes the reply to the app as an Intent. The app can then process the reply and take appropriate actions.

To use this variant, use the following values for the keys.

|       Template Key | Required | Value                                    |
| -----------------: | :------: | :--------------------------------------- |
|     pt_input_label | Required | for example, `Search`                    |
|  pt_input_feedback | Required | for example., `Thanks for your feedback` |
| pt_input_auto_open | Required | fixed value - `true`                     |

To capture the input, the app can get the `pt_input_reply` key from the Intent extras.

[block:image]
{
  "images": [
    {
      "image": [
        "https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaWithOpen.gif",
        "Input Box Template-Reply as an Intent",
        618
      ],
      "align": "center",
      "sizing": "35% ",
      "border": true,
      "caption": "Input Box Template-Reply as an Intent"
    }
  ]
}
[/block]


## Template Keys

[block:parameters]
{
  "data": {
    "h-0": "Input Box Template Keys",
    "h-1": "Required",
    "h-2": "Description",
    "0-0": "pt_id",
    "0-1": "Required",
    "0-2": "Value - `pt_input`",
    "1-0": "pt_title",
    "1-1": "Required",
    "1-2": "Title",
    "2-0": "pt_msg",
    "2-1": "Required",
    "2-2": "Message",
    "3-0": "pt_big_img",
    "3-1": "Required",
    "3-2": "Image",
    "4-0": "pt_big_img_alt",
    "4-1": "Optional",
    "4-2": "Image to be shown after feedback is collected",
    "5-0": "pt_input_label",
    "5-1": "Required",
    "5-2": "Label text to be shown on the input",
    "6-0": "pt_input_feedback",
    "6-1": "Required",
    "6-2": "Feedback",
    "7-0": "pt_dl1",
    "7-1": "Required",
    "7-2": "Deep Link",
    "8-0": "pt_msg_summary",
    "8-1": "Optional",
    "8-2": "Message line when Notification is expanded",
    "9-0": "pt_subtitle",
    "9-1": "Optional",
    "9-2": "Subtitle",
    "10-0": "pt_event_name",
    "10-1": "Optional",
    "10-2": "Name of Event to be raised",
    "11-0": "pt\\_event\\_property\\_\\<property_name_1>",
    "11-1": "Optional",
    "11-2": "Value for event property \\<property_name_1>",
    "12-0": "pt\\_event\\_property\\_\\<property_name_2>",
    "12-1": "Optional",
    "12-2": "Value for event property \\<property_name_2>",
    "13-0": "pt\\_event\\_property\\_\\<property_name_n>",
    "13-1": "Optional",
    "13-2": "Value for event property \\<property_name_n>",
    "14-0": "pt_input_auto_open",
    "14-1": "Optional",
    "14-2": "Auto open the app after feedback",
    "15-0": "pt_title_clr",
    "15-1": "Optional",
    "15-2": "Title Color in HEX",
    "16-0": "pt_title_clr_dark",
    "16-1": "Optional",
    "16-2": "Title Color in HEX in dark mode.",
    "17-0": "pt_msg_clr",
    "17-1": "Optional",
    "17-2": "Message Color in HEX",
    "18-0": "pt_msg_clr_dark",
    "18-1": "Optional",
    "18-2": "Message Color in HEX in dark mode",
    "19-0": "pt_small_icon_clr",
    "19-1": "Optional",
    "19-2": "Small Icon Color in HEX",
    "20-0": "pt_small_icon_clr_dark",
    "20-1": "Optional",
    "20-2": "Small Icon Color in HEX in dark mode",
    "21-0": "pt_ico",
    "21-1": "Optional",
    "21-2": "Large Icon",
    "22-0": "wzrk_hide_large_icon",
    "22-1": "Optional",
    "22-2": "Hide the large icon in Android notifications  \nValues:  \n  \n<li> hide = `true` to hide the icon </li> <li> hide = `false` to show the icon (Default) </li>",
    "23-0": "pt_dismiss_on_click",
    "23-1": "Optional",
    "23-2": "Dismiss notification on click",
    "24-0": "pt_json",
    "24-1": "Optional",
    "24-2": "Above keys in JSON format"
  },
  "cols": 3,
  "rows": 25,
  "align": [
    "right",
    "center",
    "left"
  ]
}
[/block]


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

> 📘 **Custom Color Definitions**
> 
> Templates support **custom color definitions** that adapt to both themes for visual consistency.

> 🚧 **SDK Version Requirement**
> 
> To use the Dark mode feature, ensure you are using Push Templates SDK version 2.1.0 and above.

# Image Scaling

Android supports various image scaling options to control how images appear in push notifications. CleverTap optimizes image rendering to maintain visual consistency across devices while leveraging Android native scaling behavior. To handle scaling in a Push template, you must add the key `pt_scale_type` key and the value is set as `fit_center` or `center_crop` based on the requirement.

The following table lists the different scaling settings in Push templates:

[block:parameters]
{
  "data": {
    "h-0": "Image Scaling Type",
    "h-1": "Description",
    "h-2": "Sample Output",
    "0-0": "**Keep Original**",
    "0-1": "Applies to all expanded images in Android push templates.  \n  \nThe image is scaled proportionally (maintaining the aspect ratio) to ensure that both its width and height fit within the boundaries of the image container. This ensures the entire image is visible without any cropping.",
    "0-2": "![](https://files.readme.io/8ee6f3ee4c83f0c5858268cca7d494fdc6492c2eaff59a7daf85d504260f365c-image.png)",
    "1-0": "**Scale to Fill**",
    "1-1": "Applies to all expanded images in all Android push templates.  \n  \nThe image is resized uniformly while preserving the aspect ratio.  If necessary, it is scaled down so that both width and height fit within the image container. This ensures the full image is displayed without any part being cropped.",
    "1-2": "![](https://files.readme.io/ca12801ffa07a814fa59f413d383b80185acb5d6c9dc36b5aa04721bc21800ae-image.png)"
  },
  "cols": 3,
  "rows": 2,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


> 🚧 **SDK Version Requirement**
> 
> To use the Image scaling feature, ensure you are using Push Templates SDK version 2.1.0 and above.

# Image Specifications

The following are the image specifications and guidelines for the Push Templates:

[block:parameters]
{
  "data": {
    "h-0": "Template",
    "h-1": "Aspect Ratios (Approx.)",
    "h-2": "Maximum File Size  \n(OS version 12 and above)",
    "0-0": "**Standard**",
    "0-1": "<li> OS version 12 and above: **3:2**</li> <li>OS version 11 and below: **5:3**</li>",
    "0-2": "",
    "1-0": "**Advanced**",
    "1-1": "<li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>",
    "1-2": "500 KB",
    "2-0": "**Auto Carousel**",
    "2-1": "<li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>",
    "2-2": "86 KB",
    "3-0": "**Manual Carousel**",
    "3-1": "<li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>",
    "3-2": "86 KB",
    "4-0": "**Five Icon**",
    "4-1": "<li> OS version 12 and above: **1:1**</li><li>OS version 11 and below: **1:1**</li>",
    "4-2": "50 KB",
    "5-0": "**Text over Image**",
    "5-1": "<li>OS version 12 and above: **1:1**</li><li>OS version 11 and below: **2:1**</li>",
    "5-2": "500 KB",
    "6-0": "**Timer**",
    "6-1": "<li>OS version 12 and above: **3:2**</li><li>OS version 11 and below: **5:3**</li>",
    "6-2": "326 KB",
    "7-0": "**Product Catalogue Linear View**",
    "7-1": "<li>OS version 12 and above - **3:2**</li><li>OS version 11 and below- **2:1**</li>",
    "7-2": "86 KB",
    "8-0": "**Product Catalogue Vertical View**",
    "8-1": "<li>OS version 12 and above - **3:4**</li><li>OS version 11 and below- **1:1**</li>",
    "8-2": "86 KB",
    "9-0": "**Input Box**",
    "9-1": "<li>OS version 12 and above - **3:2**</li><li>OS version 11 and below- **5:3**</li>",
    "9-2": "500 KB",
    "10-0": "**Ratings**",
    "10-1": "<li>OS version 12 and above - **3:1**</li><li>OS version 11 and below- **5:3**</li>",
    "10-2": "500 KB"
  },
  "cols": 3,
  "rows": 11,
  "align": [
    "right",
    "left",
    "left"
  ]
}
[/block]


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
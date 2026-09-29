---
title: iOS Rich Push Notifications
excerpt: >-
  Learn about iOS Rich Push Notifications – Enhance notifications with images,
  GIFs, and videos for a richer experience.
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

iOS 10 introduced Rich Push Notifications, allowing marketers to enhance push messages with images, videos, audio, or GIFs. These visually rich notifications enable quick, engaging interactions without requiring users to open the app, highlighting the increasing role of notifications in improving user experience.

To implement Rich Push Notifications, you must use a [Notification Service Extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension) and a [Notification Content Extension](https://developer.apple.com/documentation/usernotificationsui/unnotificationcontentextension). These are separate binaries bundled within your app. Before displaying a new push notification, the system invokes your service extension to modify the payload and attach media. The content extension, on the other hand, enables a custom UI to display the notification.

* **Notification Service Extension**: Modifies the notification before display (for example, adds media).
* **Notification Content Extension**: Provides a custom UI for the notification (for example, carousels).

# Prerequisites

Ensure the following prerequisites are set up to enable full functionality and optimal rendering of the iOS push notifications:

* iOS 10 or later
* Push Notifications enabled
* CocoaPods, Carthage, or SPM
* Xcode

# Configure Rich Push Notifications

To configure rich push notifications, perform the following steps:

1. Create a notification service extension in your Xcode project, and select _File_ > _New_ > _Target_.
2. Select the _Notification Service Extension_ from the available extensions.


<Image src="https://files.readme.io/99f83fd-ios10.png" alt="Select Notification Service Extension" align="center" caption="Select Notification Service Extension" border={true} />


3. Install [CTNotificationService](https://github.com/CleverTap/CTNotificationService#configure-your-notification-service-extension-to-use-the-ctnotificationserviceextension-class) in the **Notification Service Extension**. You can install it manually via [CocoaPods](doc:ios-quickstart-guide#install-using-swift-package-manager)(recommended) or Carthage or SPM.

Your Podfile must be similar to the following code:

```coffeescript Ruby
source 'https://github.com/CocoaPods/Specs.git'  
platform :ios, '10.0'  

use_frameworks!  

target 'YOUR_NOTIFICATION_SERVICE_TARGET_NAME' do  
    pod 'CTNotificationService'  
end  
```

4. Run the command,`pod install`.

## Configure `Notification Service Extension` to use the `CTNotificationServiceExtension` class

By default, `CTNotificationServiceExtension` searches for the push payload with default values for media URL and media type (for example, image, video, audio, or GIF).

To use these default keys, configure the `CTNotificationServiceExtension` as the value for NSExtension -> `NSExtensionPrincipalClass` in your Notification Service Extension target’s _Info.plist_ as follows:

```objectivec
NSExtension > NSExtensionPrincipalClass = CTNotificationServiceExtension
```

Alternatively, you can retain the `NSExtensionPrincipalClass` and extend `CTNotificationServiceExtension` in your `NotificationService class`. This allows you to override the default key names as required. In this case, override only the `didReceive(request:contentHandler:)` method, as shown in the example.

<Callout icon="🚧" theme="warn">
  ### Enable App Transport Security

  If you plan to support non-SSL URLs, set _Allow Arbitrary Loads_ as **true** under _App Transport Security Settings_ in your `Info. list`. Since this reduces network security, you can use it only in trusted environments.
</Callout>

# Configure Notification Content Extension

To support image captions, sub-captions, and carousel media, add [CTNotificationContent](https://github.com/CleverTap/CTNotificationContent) to your Notification Content Extension. You can install it manually or using [CocoaPods](doc:ios-quickstart-guide#install-using-swift-package-manager) (recommended), Carthage, SPM.

Your Podfile must be similar to the following:

```coffeescript Ruby
source 'https://github.com/CocoaPods/Specs.git'  
platform :ios, '10.0'  

use_frameworks!  

target 'YOUR_NOTIFICATION_CONTENT_TARGET_NAME' do  
    pod 'CTNotificationContent'  
end  
```

To configure Notification Content Extension, perform the following steps:

1. Create a Notification Content Extension in your Xcode project, select _File_ > _New_ > _Target_ and select the _Notification Content Extension_ template.


<Image src="https://files.readme.io/899cf2d-content_extension.png" alt="1462" align="center" caption="Notification Content Extension Template" border={true} />


2. Run `pod install`.

To configure your Notification Content Extension to use the `CTNotificationViewController` class, perform the following steps:

1. Set the superclass of your `NotificationViewController` to `CTNotificationViewController`. You need not implement any methods from the `UNNotificationContentExtension` protocol; `CTNotificationViewController` handles them automatically. For more information, refer to [Objective-C example](https://github.com/CleverTap/CTNotificationContent/tree/master/Example/NotificationContent) and [Swift example](https://github.com/CleverTap/CTNotificationContent/tree/master/ExampleSwift/NotificationContent).
2. Edit the `MainInterface.storyboard` file in your Notification Content Extension target and replace the default view with a plain `UIView`.

```swift
// register category with actions
    let action1 = UNNotificationAction(identifier: "action_1", title: "Back", options: [])
    let action2 = UNNotificationAction(identifier: "action_2", title: "Next", options: [])
    let action3 = UNNotificationAction(identifier: "action_3", title: "View In App", options: [])
    let category = UNNotificationCategory(identifier: "CTNotification", actions: [action1, action2, action3], intentIdentifiers: [], options: [])
    UNUserNotificationCenter.current().setNotificationCategories([category])
```
```objectivec
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
    UNNotificationAction *action1 = [UNNotificationAction actionWithIdentifier:@"action_1" title:@"Back" options:UNNotificationActionOptionNone];
    UNNotificationAction *action2 = [UNNotificationAction actionWithIdentifier:@"action_2" title:@"Next" options:UNNotificationActionOptionNone];
    UNNotificationAction *action3 = [UNNotificationAction actionWithIdentifier:@"action_3" title:@"View In App" options:UNNotificationActionOptionNone];
    UNNotificationCategory *cat = [UNNotificationCategory categoryWithIdentifier:@"CTNotification" actions:@[action1, action2, action3] intentIdentifiers:@[] options:UNNotificationCategoryOptionNone];
    [center setNotificationCategories:[NSSet setWithObjects:cat, nil]];
```

3. Register your notification category in your `AppDelegate`.
4. Update Notification Content target _Info.plist_ with the following configurations:
   * Set `NSExtension -> NSExtensionAttributes -> UNNotificationExtensionCategory` to match the category identifier you registered (for example, `CTNotification`).
   * Set `UNNotificationExtensionInitialContentSizeRatio` to `0.1`.
   * Set `UNNotificationExtensionDefaultContentHidden` to `true`.

For more information, refer to  [Sample plist](https://github.com/CleverTap/CTNotificationContent/blob/master/Example/NotificationContent/Info.plist).

<Callout icon="🚧" theme="warn">
  ### Downloading Media from Non-SSL URLs

  If you plan to support non-SSL URLs, set _Allow Arbitrary Loads_ as **true** under _App Transport Security Settings_ in your `Info.plist`. You can use this only in trusted environments, since it reduces network security.
</Callout>

# Send Rich Push Notifications from Dashboard

To create a Rich Push Notification campaign on the CleverTap dashboard, follow the steps below:

1. Under the **WHAT** section, enter the required values in the **Title** and **Message** .


<Image src="https://files.readme.io/5e7cbe693f836e822ffc6bcf7e0b5fb8d00446e970a50044d4c926b5808e5962-image.png" alt="Configure What Section" align="center" caption="Configure What Section" border={true} />


2. Under _Rich media_, select either _Single Image_ or _Carousel_.

<Callout icon="🚧" theme="warn">
  ### Required Category Configuration

  The category used while creating the campaign must match the category configured in the Notification Content target’s `Info.plist`. This step is mandatory.
</Callout>


<Image src="https://files.readme.io/e5a8b402b185d436ca8feafb52a4ea58e8bec8775c226158606d5d515a250cb7-image.png" alt="Configure Rich Push Notifications" align="center" caption="Configure Rich Push Notifications" border={true} />


<Callout icon="📘" theme="info">
  ### Custom Key-Value Pair

  You can choose to add a custom key-value pair separately. To add the template keys separately or into one JSON object, use the `pt_json` key to fill in the values. For more information on template keys, refer to [Template Keys](https://github.com/CleverTap/CTNotificationContent#template-keys).

  When using the CleverTap Server API to send push, include the **mutable-content**: **true** key-value pair in the platform-specific: iOS section of the request payload. For more information, refer to [Key-Value Pairs](doc:create-campaign-api#body-parameters---target-user-events--properties).
</Callout>

4. Schedule the Push Notification.

# Rich Push Templates

Rich Push Templates enable you to enhance standard push notifications with engaging visuals and interactive elements. These templates help capture user attention and improve conversion by delivering more immersive messaging experiences.

## Rich Media

Rich Media templates allow you to include multimedia elements such as images, videos, and carousels within your push notifications. These visually appealing formats highlight key content and drive user interaction.

### Single Image

The Single Media template is designed for a focused notification experience. It displays a single image alongside your notification content, enhancing user engagement by providing a visually rich preview that complements the message. This template is ideal for promotions, announcements, product highlights, or event reminders.


<Image src="https://files.readme.io/3c922ed781e4d576e5e37148b0e07044065e95a99a21dd716993a63721ce9058-image.png" alt="Rich Media - Single Image Template" align="center" width="35% " caption="Rich Media - Single Image Template" border={true} />


### Content Slider

The Content Slider template enables a dynamic and interactive notification experience by allowing users to swipe through a carousel of images, each with its unique messaging and call-to-action. This rich push format is ideal for showcasing multiple offers, features, or updates in a single notification, providing a multi-frame storytelling capability within the native iOS notification interface.


<Image src="https://files.readme.io/d0f96e99f611aa88494ae96e9ec21e16b99345caa08928701c926501bdf47eeb-ContentSlider_Video_1.gif" alt="Rich Media- Content Slider Template" align="center" width="35% " caption="Rich Media- Content Slider Template" border={true} />


## Other Templates

Other Templates offer flexible notification formats beyond rich media, allowing you to tailor your messages for different use cases such as simple alerts, quick updates, or branded announcements.

### Advanced Template

The Advanced Template is the most versatile notification format. It delivers an image-based message with optional text and background color customization. It is ideal for simple alerts, updates, or announcements that prioritize clarity and speed of delivery while still offering a branded look and feel through color customization.


<Image src="https://files.readme.io/9b13568b7a332c96bf662eb7be98d65a674ac67ec8be93325f28c94bed89d039-image.png" alt="Advanced Template" align="center" width="35% " caption="Advanced Template" border={true} />


### Auto Carousel Template

The Auto Carousel template delivers an engaging and visually dynamic Push Notification through an automatic image slider. Unlike the manual content slider, this version cycles through multiple images automatically, drawing user attention without requiring interaction. Each slide can include custom text and background color options, directly allowing for a branded, animated storytelling format in the notification tray.


<Image src="https://files.readme.io/4f0ade9accc6420cf683e1fbdfbb8e7b30bcf3686d1e387c981e6e789c31e875-autocarousel_video_1.gif" alt="Auto Carousel Template" align="center" width="35% " caption="Auto Carousel Template" border={true} />


### Manual Carousel Template

The Manual Carousel template allows users to manually swipe or tap through images using the **Next** and **Back** buttons. Each image (slide) includes a title, subtitle, and optional action.

Unlike the Auto Carousel, this version allows users to navigate the content at their own pace, making it ideal for more detailed or exploratory messages.


<Image src="https://files.readme.io/be9ecc9c923c45cf5922ffd0b8894c1cc31e65c56fc14c1d0949e9f0362f8b37-manualcarousel_video_1.gif" alt="Manual Carousel Template" align="center" width="35% " caption="Manual Carousel Template" border={true} />


#### Template Keys

| Key                 | Required | Description                                                                                                               |
| :------------------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| `pt_id`             | Required | Template type identifier. Value: `pt_manual_carousel`.                                                                    |
| `pt_title`          | Required | Notification title.                                                                                                       |
| `pt_msg`            | Required | Notification message.                                                                                                     |
| `pt_msg_summary`    | Optional | Message line shown when the notification is expanded.                                                                     |
| `pt_dl1`            | Required | Deep link for the first image. Used as the fallback deep link if `pt_dl2` or `pt_dl3` are not set.                        |
| `pt_dl2`            | Optional | Deep link for the second image. Falls back to `pt_dl1` if not set. Available from CTNotificationContent v1.5.1 and above. |
| `pt_dl3`            | Optional | Deep link for the third image. Falls back to `pt_dl1` if not set. Available from CTNotificationContent v1.5.1 and above.  |
| `pt_img1`           | Required | URL of the first image.                                                                                                   |
| `pt_img1_alt_text`  | Optional | Accessibility alt text for the first image.                                                                               |
| `pt_img2`           | Required | URL of the second image.                                                                                                  |
| `pt_img2_alt_text`  | Optional | Accessibility alt text for the second image.                                                                              |
| `pt_img3`           | Required | URL of the third image.                                                                                                   |
| `pt_img3_alt_text`  | Optional | Accessibility alt text for the third image.                                                                               |
| `pt_bg`             | Optional | Background color in HEX.                                                                                                  |
| `pt_bg_dark`        | Optional | Background color for dark mode in HEX.                                                                                    |
| `pt_title_clr`      | Optional | Title color in HEX.                                                                                                       |
| `pt_msg_clr`        | Optional | Message color in HEX.                                                                                                     |
| `pt_title_clr_dark` | Optional | Title color for dark mode in HEX.                                                                                         |
| `pt_msg_clr_dark`   | Optional | Message color for dark mode in HEX.                                                                                       |
| `pt_json`           | Optional | All of the above keys in JSON format.                                                                                     |

### Timer Template

This template features a live countdown timer. After the timer expires, you can show a different title, message, and background image.


<Image src="https://files.readme.io/3b75c1c4f746501277267c9945b7f40dc480ab39e9a202c4ee1dc5f49f217cfe-timer_video_1.gif" alt="Timer Template" align="center" width="35% " caption="Timer Template" border={true} />


If the image is not downloaded, the template falls back to the basic template with only the Title and Message.

<Callout icon="👍" theme="okay">
  ### Theme-based Rendition

  Push Notification appears dark or light based on the following settings in your iOS device:

  * **Dark mode is enabled**- Notification renders in dark background.
  * **Dark mode is disabled** - Notification renders in light background.

  The templates also support custom color definitions that adapt to light and dark themes, ensuring visual consistency across modes.
</Callout>

# GIF Support

CleverTap supports GIFs in push templates starting with SDK version 1.2.0 on devices running iOS 10 or later. This allows you to add animated media to push notifications, enhancing user engagement through dynamic visuals.


<Image src="https://files.readme.io/97fee5e125d68f4efa6fd5c3010686239475d9d79a86a4b53f24972b0772244d-Screen_Recording_2026-03-05_at_4.11.41_PM_1.gif" align="center" width="35% " border={true} />


## Specifications and Design Guidelines

* Supported on iOS-  with SDK v1.2.0 or later.
* For older iOS versions or SDKs, CleverTap shows the first frame of the GIF as a static image.
* Use the recommended aspect ratio for the selected template to avoid cropping or distortion.
* Keep file size under 1MB for smooth rendering.
* Opt for looped, visually clear GIFs that work well even if partially cropped.

## Limitations

* Devices running iOS 9 and below do not support GIF playback; a static fallback image is shown.
* Some OEMs may not fully support GIF rendering, even on iOS 10 and later.

## Use HTML Tags Support in Rich Push Notification Titles and Messages

Starting with v0.3.0, `CTNotificationContent` supports bold, italic, underline, and strikethrough HTML tags in **Title** and **Message** for rich push notifications. This allows you to format text and add visual emphasis to your rich push notifications.


<Image src="https://files.readme.io/43b599b4f2e0d08ef65f92d2a53297f186e559e81d03e4bc769948a2b74a1eb3-Simulator_Screenshot_-_iPhone_16_Pro_-_2025-04-10_at_17.40.19.png" alt="HTML Tags" align="center" width="250px" caption="HTML Tags" border={true} />


The following is a sample APN payload using the `pt_msg` key with HTML tags. The message supports bold, italic, underline, and font color styling.

```swift
{
    "Simulator Target Bundle": "com.clevertap.static",
    "W$dt": "APPLE",
    "W$id": "0_0",
    "W$pivot": "wzrk_default",
    "W$rnv" : 0,
    "aps":  {
        "alert" :         {
            "body" : "my message",
            "title" : "my title",
        },
        "badge":42,
        "sound":"default",
        "category":"CTNotification",
        "mutable-content": 1
    },
    "pt_id" : "pt_basic",
    "pt_json" : "{\"pt_title\":\"my message is <b> bold </b>, <i> italic </i> and <u> underline </u>\",\"pt_dl1\":\"https://www.google.com\",\"pt_big_img\":\"https://ichef.bbci.co.uk/news/976/cpsprodpb/C448/production/_117684205_lotus.jpg\",\"pt_bg\":\"#F3F3F3\",\"pt_msg_clr\":\"#FF0000\"}",
    "wzrk_acct_id" : "test-account-id"
}
```

# Integrate Push Templates: Examples and Usage

Use the following code samples to learn how to integrate and use Push templates:

* [Swift project](https://github.com/CleverTap/CTNotificationContent/tree/master/ExampleSwift)
* [Objective-C project](https://github.com/CleverTap/CTNotificationContent/tree/master/Example)

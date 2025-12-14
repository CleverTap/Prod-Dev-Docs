---
title: Android Push Templates
excerpt: >-
  Learn how to implement and customize Push Notification for your Android
  device.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: android-in-app-notifications
      title: Android In-App
    - type: basic
      slug: android-app-inbox
      title: Android App Inbox
    - type: basic
      slug: android-native-display
      title: Android Native Display
    - type: basic
      slug: multi-instance-support-android
      title: Multi-Instance Support Android
    - type: basic
      slug: android-advanced-settings
      title: Android Advanced Settings
    - type: basic
      slug: android-resources
      title: Android Resources
---
# Push Templates by CleverTap

CleverTap Push Templates SDK helps you engage with your users using fancy push notification templates built specifically to work with CleverTap Android SDK v4.4.0 and above.

# Installation

### Out of the box

1. Add the dependencies to the `build.gradle`:

```groovy
implementation "com.clevertap.android:push-templates:1.3.0"
implementation "com.clevertap.android:clevertap-android-sdk:7.3.1" // 4.4.0 and above
```

2. Add the following line to your Application class.

```java
CleverTapAPI.setNotificationHandler((NotificationHandler)new PushTemplateNotificationHandler());
```
```kotlin
CleverTapAPI.setNotificationHandler(PushTemplateNotificationHandler() as NotificationHandler);
```

### Custom Handling Push Notifications

Add the following code in your custom `FirebaseMessageService` class:

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

### Migration from v0.0.8 to v1.0.0 and above

Remove the following Receivers and Services from your `AndroidManifest.xml` and follow the steps given above:

```xml
<service
    android:name="com.clevertap.pushtemplates.PushTemplateMessagingService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>

<service
    android:name="com.clevertap.pushtemplates.PTNotificationIntentService"
    android:exported="false">
        <intent-filter>
            <action android:name="com.clevertap.PT_PUSH_EVENT"/>
        </intent-filter>
</service>

<receiver
    android:name="com.clevertap.pushtemplates.PTPushNotificationReceiver"
    android:exported="false"
    android:enabled="true">
</receiver>

<receiver
    android:name="com.clevertap.pushtemplates.PushTemplateReceiver"
    android:exported="false"
    android:enabled="true">
</receiver>
```

# Dashboard Usage

While creating a Push Notification campaign on CleverTap, just follow the steps below:

1. On the *WHAT* section, pass the desired values in the *title* and *message* fields.

> 📘 Key-value pair
>
> We prioritize title and message provided in the key-value pair - as shown in *step 2* of Installation.

<Image title="Configure What Section" alt={982} align="center" border={true} src="https://files.readme.io/6faeb0d-basic.png">
  Configure What Section
</Image>

2. Click on **Advanced** and then click on **Add pair** to add the Template Keys.

<Image title="Add Key Value Pairs" alt={2638} align="center" border={true} src="https://files.readme.io/f85ab3a-kv.png">
  Add Key Value Pairs
</Image>

3. You can also add the above keys into one JSON object and use the `pt_json` key to fill in the values.

<Image title="Add Key into JSON Object" alt={1872} align="center" border={true} src="https://files.readme.io/d456b21-json.png">
  Add Key into JSON Object
</Image>

4. Send a test push and schedule.

# Template Types

## Basic Template

Basic Template is the basic push notification received on apps.

<Image title="Basic Push Notification Template" alt={624} align="center" width="70% " border={true} src="https://files.readme.io/2db4575-basic_color.png">
  Basic Push Notification Template
</Image>

## Auto Carousel Template

Auto carousel is an automatic revolving carousel push notification.

<Image title="Auto Carousel Template" alt={624} align="center" width="70% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/autocarouselv0.0.3.gif">
  Auto Carousel Template
</Image>

## Manual Carousel Template

This is the manual version of the carousel. The user can navigate to the next image by clicking on the arrows.

<Image title="Manual Carousel Template" alt={624} align="center" width="70% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/manual.gif">
  Manual Carousel Template
</Image>

If only one image can be downloaded, this template falls back to the *Basic Template*.

### Filmstrip Variant

The manual carousel has an extra variant called `filmstrip`. This can be used by adding the following key-value:

|               Template Key | Required | Value       |
| -------------------------: | :------: | :---------- |
| pt\_manual\_carousel\_type | Optional | `filmstrip` |

(Expanded and unexpanded example)

<Image title="Filmstrip Variant" alt={624} align="center" width="70% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/filmstrip.gif">
  Filmstrip Variant
</Image>

## Rating Template

Rating template lets your users give you feedback. This feedback is captured in the event *Rating Submitted* within the property `wzrk_c2a`.

<Image title="Rating Template" alt={624} align="center" width="70% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/rating.gif">
  Rating Template
</Image>

## Product Catalog Template

The product catalog template lets you showcase different product images (or a product catalog) before the user can click on the **BUY NOW** option, which can take them directly to the product via deep links. This template has two variants.

### Vertical View

<Image title="Product Catalog Template-Vertical View" alt={624} align="center" width="70% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/productdisplay.gif">
  Product Catalog Template-Vertical View
</Image>

### Linear View

Use the following keys to enable the linear view variant of this template:

|                 Template Key | Required | Value  |
| ---------------------------: | :------: | :----- |
| pt\_product\_display\_linear | Optional | `true` |

<Image title="Product Catalog Template-Linear View" alt={624} align="center" width="70% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/proddisplaylinear.gif">
  Product Catalog Template-Linear View
</Image>

## Five Icons Template

The Five Icons template presents a push notification without text, featuring only five icons. With a single click, users can conveniently access their preferred functionality. If at least three icons are not retrieved from the source, the library does not render any notification. 

The details of each Call-to-Action (CTA) are captured in the *Notification Clicked* event within the property *wzrk\_c2a*. If the user clicks on any notification area except the five icons, an activity intent is launched by default.

<Image title="Five Icons Template" alt="Sample Five Icon Template" align="center" border={true} src="https://files.readme.io/37082db6b326b5d73c37c3f6211424ff34c1f9ceef892899ea4876e8a691aec3-Five_Icon.png">
  Five Icons Template
</Image>

## Timer Template

This template features a live countdown timer. You can even choose to show different titles, messages, and background images after the timer expire.

Timer notification is only supported for Android N (7) and above. For OS versions below N, the library falls back to the *Basic Template*.

<Image title="Timer Template" alt={618} align="center" border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/timer.gif">
  Timer Template
</Image>

## Zero Bezel Template

The Zero Bezel template ensures that the background image covers the entire available surface area of the push notification. All the text is overlayed on the image.

The library will fall back to the Basic Template if the image can't be downloaded.

<Image title="Zero Bezel Template" alt={618} align="center" border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/zerobezel.gif">
  Zero Bezel Template
</Image>

## Input Box Template

The Input Box Template lets you collect any input, including feedback from your users. It has four variants.

### With CTAs

The CTA variant of the Input Box Template uses action buttons on the notification to collect input from the user.

To set the CTAs, use the Advanced Options when setting up the campaign on the dashboard.

<Image title="Input Box Template-With CTAs" alt={618} align="center" width="50% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputctabasicdismiss.gif">
  Input Box Template-With CTAs
</Image>

|           Template Key | Required | Value                                                        |
| ---------------------: | :------: | :----------------------------------------------------------- |
| pt\_dismiss\_on\_click | Optional | `true` (Dismisses the notification without opening the app). |

> 📘 Display the Notification in the tray (for Android 12 and above)
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

### CTAs with Remind Later option

This variant of the Input Box Template is advantageous if the user wants to be reminded of the notification after some time. Clicking on the remind later button raises an event to the user profiles, with a custom user property p2, whose value is a future timestamp. You can have a campaign running on the dashboard that will send a reminder notification at the timestamp in the event property.

Ensure to set one of the CTAs as a Remind Later button. Set the action ID to `remind` from the dashboard.

|                              Template Key | Required | Value                                                                                                                                                                  |
| ----------------------------------------: | :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                           pt\_event\_name | Required | For example: `Remind Later`,                                                                                                                                           |
| pt\_event\_property\_\<property\_name\_1> | Optional | For example: `<property_value>`,                                                                                                                                       |
| pt\_event\_property\_\<property\_name\_2> | Optional | future epoch timestamp. For example: `\$D_1592503813`                                                                                                                  |
|                    pt\_dismiss\_on\_click | Required | Value should be `true`. It dismisses the notification without opening the app and raises a required event to the user profile, needed to send a reminder notification. |

<Image title="Input Box Template-CTAs with Remind Later option" alt={618} align="center" width="50% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaRemind.gif">
  Input Box Template-CTAs with Remind Later option
</Image>

### Reply as an Event

This variant raises an event capturing the user's input as an event property. The app is not opened after the user sends the reply.

To use this variant, use the following values for the keys.

|                              Template Key | Required | Value                                |
| ----------------------------------------: | :------: | :----------------------------------- |
|                          pt\_input\_label | Required | for e.g., `Search`                   |
|                       pt\_input\_feedback | Required | for e.g., `Thanks for your feedback` |
|                           pt\_event\_name | Required | for e.g. `Searched`,                 |
| pt\_event\_property\_\<property\_name\_2> | Required | fixed value - `pt_input_reply`       |
| pt\_event\_property\_\<property\_name\_1> | Optional | for e.g. `<property_value>`,         |

<Image title="Input Box Template-Reply as an Event" alt={618} align="center" width="50% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaNoOpen.gif">
  Input Box Template-Reply as an Event
</Image>

### Reply as an Intent

This variant passes the reply to the app as an Intent. The app can then process the reply and take appropriate actions.

To use this variant, use the following values for the keys.

|          Template Key | Required | Value                                |
| --------------------: | :------: | :----------------------------------- |
|      pt\_input\_label | Required | for e.g., `Search`                   |
|   pt\_input\_feedback | Required | for e.g., `Thanks for your feedback` |
| pt\_input\_auto\_open | Required | fixed value - `true`                 |

To capture the input, the app can get the `pt_input_reply` key from the Intent extras.

<Image title="Input Box Template-Reply as an Intent" alt={618} align="center" width="50% " border={true} src="https://github.com/CleverTap/clevertap-android-sdk/raw/master/static/inputCtaWithOpen.gif">
  Input Box Template-Reply as an Intent
</Image>

# Template Keys

### Basic Template

|  Basic Template Keys | Required | Description                                |
| -------------------: | :------: | :----------------------------------------- |
|               pt\_id | Required | Value - `pt_basic`                         |
|            pt\_title | Required | Title                                      |
|              pt\_msg | Required | Message                                    |
|               pt\_bg | Required | Background Color in HEX                    |
|     pt\_msg\_summary | Required | Message line when Notification is expanded |
|         pt\_subtitle | Optional | Subtitle                                   |
|         pt\_big\_img | Optional | Image                                      |
|              pt\_ico | Optional | Large Icon                                 |
|              pt\_dl1 | Optional | One Deep Link (minimum)                    |
|       pt\_title\_clr | Optional | Title Color in HEX                         |
|         pt\_msg\_clr | Optional | Message Color in HEX                       |
| pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                    |
|             pt\_json | Optional | Above keys in JSON format                  |

### Auto Carousel Template

| Auto Carousel Template Keys | Required | Description                                |
| --------------------------: | :------: | :----------------------------------------- |
|                      pt\_id | Required | Value - `pt_carousel`                      |
|                   pt\_title | Required | Title                                      |
|                     pt\_msg | Required | Message                                    |
|                     pt\_dl1 | Required | Deep Link (Max one)                        |
|                    pt\_img1 | Required | Image One                                  |
|                    pt\_img2 | Required | Image Two                                  |
|                    pt\_img3 | Required | Image Three                                |
|                      pt\_bg | Required | Background Color in HEX                    |
|            pt\_msg\_summary | Optional | Message line when Notification is expanded |
|                pt\_subtitle | Optional | Subtitle                                   |
|                  pt\_img`n` | Optional | Image `N`                                  |
|                     pt\_ico | Optional | Large Icon                                 |
|              pt\_title\_clr | Optional | Title Color in HEX                         |
|                pt\_msg\_clr | Optional | Message Color in HEX                       |
|        pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                    |
|                    pt\_json | Optional | Above keys in JSON format                  |

### Manual Carousel Template

| Manual Carousel Template Keys | Required | Description                                |
| ----------------------------: | :------: | :----------------------------------------- |
|                        pt\_id | Required | Value - `pt_manual_carousel`               |
|                     pt\_title | Required | Title                                      |
|                       pt\_msg | Required | Message                                    |
|                       pt\_dl1 | Required | Deep Link One                              |
|                      pt\_img1 | Required | Image One                                  |
|                      pt\_img2 | Required | Image Two                                  |
|                      pt\_img3 | Required | Image Three                                |
|                        pt\_bg | Required | Background Color in HEX                    |
|              pt\_msg\_summary | Optional | Message line when Notification is expanded |
|                  pt\_subtitle | Optional | Subtitle                                   |
|                       pt\_dl2 | Optional | Deep Link Two                              |
|                     pt\_dl`n` | Optional | Deep Link for the nth image                |
|                    pt\_img`n` | Optional | Image `N`                                  |
|                       pt\_ico | Optional | Large Icon                                 |
|                pt\_title\_clr | Optional | Title Color in HEX                         |
|                  pt\_msg\_clr | Optional | Message Color in HEX                       |
|          pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                    |
|                      pt\_json | Optional | Above keys in JSON format                  |
|    pt\_manual\_carousel\_type | Optional | `filmstrip`                                |

### Rating Template

| Rating Template Keys | Required | Description                                |
| -------------------: | :------: | :----------------------------------------- |
|               pt\_id | Required | Value - `pt_rating`                        |
|            pt\_title | Required | Title                                      |
|              pt\_msg | Required | Message                                    |
|      pt\_default\_dl | Required | Default Deep Link for Push Notification    |
|              pt\_dl1 | Required | Deep Link for first/all star(s)            |
|               pt\_bg | Required | Background Color in HEX                    |
|     pt\_msg\_summary | Optional | Message line when Notification is expanded |
|         pt\_subtitle | Optional | Subtitle                                   |
|              pt\_dl2 | Optional | Deep Link for second star                  |
|              pt\_dl3 | Optional | Deep Link for third star                   |
|              pt\_dl4 | Optional | Deep Link for fourth star                  |
|              pt\_dl5 | Optional | Deep Link for fifth star                   |
|              pt\_ico | Optional | Large Icon                                 |
|       pt\_title\_clr | Optional | Title Color in HEX                         |
|         pt\_msg\_clr | Optional | Message Color in HEX                       |
| pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                    |
|             pt\_json | Optional | Above keys in JSON format                  |
|         pt\_big\_img | Optional | Image                                      |

### Product Catalog Template

|     Product Catalog Template Keys | Required | Description                             |
| --------------------------------: | :------: | :-------------------------------------- |
|                            pt\_id | Required | Value - `pt_product_display`            |
|                           pt\_msg | Required | Message                                 |
|                         pt\_title | Required | Title                                   |
|                          pt\_img1 | Required | Image One                               |
|                          pt\_img2 | Required | Image Two                               |
|                          pt\_img3 | Required | Image Three                             |
|                           pt\_bt1 | Required | Big text for first image                |
|                           pt\_bt2 | Required | Big text for second image               |
|                           pt\_bt3 | Required | Big text for third image                |
|                           pt\_st2 | Required | Small text for second image             |
|                           pt\_st1 | Required | Small text for first image              |
|                           pt\_st3 | Required | Small text for third image              |
|                           pt\_dl1 | Required | Deep Link for first image               |
|                           pt\_dl2 | Required | Deep Link for second image              |
|                           pt\_dl3 | Required | Deep Link for third image               |
|                        pt\_price1 | Required | Price for first image                   |
|                        pt\_price2 | Required | Price for second image                  |
|                        pt\_price3 | Required | Price for third image                   |
|                            pt\_bg | Required | Background Color in HEX                 |
|      pt\_product\_display\_action | Required | Action Button Label Text                |
| pt\_product\_display\_action\_clr | Required | Action Button Background Color in HEX   |
|                      pt\_subtitle | Optional | Subtitle                                |
|      pt\_product\_display\_linear | Optional | Linear Layout Template ("true"/"false") |
|                    pt\_title\_clr | Optional | Title Color in HEX                      |
|                      pt\_msg\_clr | Optional | Message Color in HEX                    |
|              pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                 |
|                          pt\_json | Optional | Above keys in JSON format               |

### Five Icons Template

| Five Icons Template Keys | Required | Description               |
| -----------------------: | :------: | :------------------------ |
|                   pt\_id | Required | Value - `pt_five_icons`   |
|                 pt\_img1 | Required | Icon One                  |
|                 pt\_img2 | Required | Icon Two                  |
|                 pt\_img3 | Required | Icon Three                |
|                  pt\_dl1 | Required | Deep Link for first icon  |
|                  pt\_dl2 | Required | Deep Link for second icon |
|                  pt\_dl3 | Required | Deep Link for third icon  |
|                   pt\_bg | Required | Background Color in HEX   |
|                 pt\_img4 | Optional | Icon Four                 |
|                 pt\_img5 | Optional | Icon Five                 |
|                  pt\_dl4 | Optional | Deep Link for fourth icon |
|                  pt\_dl5 | Optional | Deep Link for fifth icon  |
|     pt\_small\_icon\_clr | Optional | Small Icon Color in HEX   |
|                 pt\_json | Optional | Above keys in JSON format |

### Timer Template

|    Timer Template Keys | Required | Description                                                                                                                   |
| ---------------------: | :------: | :---------------------------------------------------------------------------------------------------------------------------- |
|                 pt\_id | Required | Value - `pt_timer`                                                                                                            |
|              pt\_title | Required | Title                                                                                                                         |
|                pt\_msg | Required | Message                                                                                                                       |
|                pt\_dl1 | Required | Deep Link                                                                                                                     |
|                 pt\_bg | Required | Background Color in HEX                                                                                                       |
|   pt\_timer\_threshold | Required | Timer duration in seconds (minimum 10)                                                                                        |
|         pt\_title\_alt | Optional | Title to show after timer expires                                                                                             |
|           pt\_msg\_alt | Optional | Message to show after timer expires                                                                                           |
|           pt\_subtitle | Optional | Subtitle                                                                                                                      |
|       pt\_msg\_summary | Optional | Message line when Notification is expanded                                                                                    |
|           pt\_big\_img | Optional | Image                                                                                                                         |
|      pt\_big\_img\_alt | Optional | Image to show when timer expires                                                                                              |
| pt\_chrono\_title\_clr | Optional | Color for timer text in HEX                                                                                                   |
|         pt\_timer\_end | Optional | Epoch Timestamp to countdown to (for example, $D\_1595871380 or 1595871380). Not needed if pt\_timer\_threshold is specified. |
|         pt\_title\_clr | Optional | Title Color in HEX                                                                                                            |
|           pt\_msg\_clr | Optional | Message Color in HEX                                                                                                          |
|   pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                                                                                                       |
|               pt\_json | Optional | Above keys in JSON format                                                                                                     |

### Zero Bezel Template

| Zero Bezel Template Keys | Required | Description                                      |
| -----------------------: | :------: | :----------------------------------------------- |
|                   pt\_id | Required | Value - `pt_zero_bezel`                          |
|                pt\_title | Required | Title                                            |
|                  pt\_msg | Required | Message                                          |
|             pt\_big\_img | Required | Image                                            |
|         pt\_msg\_summary | Optional | Message line when Notification is expanded       |
|             pt\_subtitle | Optional | Subtitle                                         |
|          pt\_small\_view | Optional | Select text-only small view layout (`text_only`) |
|                  pt\_dl1 | Optional | Deep Link                                        |
|           pt\_title\_clr | Optional | Title Color in HEX                               |
|             pt\_msg\_clr | Optional | Message Color in HEX                             |
|     pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                          |
|                  pt\_ico | Optional | Large Icon                                       |
|                 pt\_json | Optional | Above keys in JSON format                        |

### Input Box Template

|                   Input Box Template Keys | Required | Description                                   |
| ----------------------------------------: | :------: | :-------------------------------------------- |
|                                    pt\_id | Required | Value - `pt_input`                            |
|                                 pt\_title | Required | Title                                         |
|                                   pt\_msg | Required | Message                                       |
|                              pt\_big\_img | Required | Image                                         |
|                          pt\_input\_label | Required | Label text to be shown on the input           |
|                       pt\_input\_feedback | Required | Feedback                                      |
|                                   pt\_dl1 | Required | Deep Link                                     |
|                          pt\_msg\_summary | Optional | Message line when Notification is expanded    |
|                              pt\_subtitle | Optional | Subtitle                                      |
|                         pt\_big\_img\_alt | Optional | Image to be shown after feedback is collected |
|                           pt\_event\_name | Optional | Name of Event to be raised                    |
| pt\_event\_property\_\<property\_name\_1> | Optional | Value for event property \<property\_name\_1> |
| pt\_event\_property\_\<property\_name\_2> | Optional | Value for event property \<property\_name\_2> |
| pt\_event\_property\_\<property\_name\_n> | Optional | Value for event property \<property\_name\_n> |
|                     pt\_input\_auto\_open | Optional | Auto open the app after feedback              |
|                            pt\_title\_clr | Optional | Title Color in HEX                            |
|                              pt\_msg\_clr | Optional | Message Color in HEX                          |
|                      pt\_small\_icon\_clr | Optional | Small Icon Color in HEX                       |
|                                   pt\_ico | Optional | Large Icon                                    |
|                    pt\_dismiss\_on\_click | Optional | Dismiss notification on click                 |
|                                  pt\_json | Optional | Above keys in JSON format                     |

> 📘 Note
>
> `pt_title` and `pt_msg` in all the templates support HTML elements like bold `<b>`, italics `<i>` and underline `<u>`

# Developer Notes

* Using images of 3 MB or lower is recommended for better performance under Android 11.
* A silent notification channel with importance: `HIGH` is created every time on an interaction with the Rating, Manual Carousel, and Product Catalog templates with a silent sound file. This prevents the notification sound from playing when the notification is re-rendered.
* The silent notification channel is deleted whenever the notification is dismissed or clicked.
* For Android 11 and Android 12, please use images that are less than 100kb else notifications will not be rendered as advertised.
* Due to Android 12 trampoline restrictions, the Input Box template with auto open of deep link feature will fall back to simply raising the event for a reply.

## Image Specifications

> 📘 Template Rendering
>
> The templates render differently depending on the device and the version of the operating system.

The following are the image specifications for the Push Templates:

|                  Template |                    Aspect Ratios                   | File Type    | Maximum File Size |
| ------------------------: | :------------------------------------------------: | :----------- | :---------------- |
|                     Basic |                     4:3 or 2:1                     | .JPG         | 500 KB            |
|             Auto Carousel | 2:1 (Android 11 and 12) and 4:3 (Below Android 11) | .JPG         | 86 KB             |
|           Manual Carousel | 2:1 (Android 11 and 12) and 4:3 (Below Android 11) | .JPG         | 86 KB             |
| Manual Carousel-FilmStrip |                         1:1                        | .JPG         | 86 KB             |
|                    Rating | 4:3 (Android 11 and 12) and 2:1 (Below Android 11) | .JPG         | 5 MB              |
|           Product Catalog |                         1:1                        | .JPG         | 80 KB             |
|                 Five Icon |                         1:1                        | .JPG or .PNG | 50 KB             |
|                Zero Bezel |                     4:3 or 2:1                     | .JPG         | 500 KB            |
|                     Timer |                     4:3 or 2:1                     | .JPG         | 326 KB            |
|                 Input Box |                     4:3 or 2:1                     | .JPG         | 500 KB            |

### Additional Guidelines

* **Auto and Manual Carousel**: Images must not exceed 850x425 pixels for Android 11 and Android 12 devices and must have a 2:1 aspect ratio.
* **Auto and Manual Carousel**: Images must not exceed 400x200 pixels for Android 13+ devices.
* **Five Icons**: Images must not exceed 300x300 pixels for Android 13+ devices.
* **Zero Bezel**: If the image contains text, it must be centered for optimal display on Android 12+ devices.
* **Product Catalog**: Images must have a 1:1 aspect ratio and a file size of 80 KB or less. Images must not exceed 450x450 pixels for Android 13+ devices.
* **Recommendation for Android 12+ Devices**: Ensure any text within images is centered for better visibility.
* **Recommendation for Android 13+ Devices**: Ensure images must not exceed 450x450 px for optimal rendering.

## Android 12 Trampoline restrictions

With Android 12, the Rating and Product Display template push notifications do not get dismissed once the deep link is opened.

To handle this, you'll have to add the following code to the `onActivityResumed` or `onNewIntent` of your app.

```java
Bundle payload = activity.getIntent().getExtras();
    if (payload.containsKey("pt_id")&& payload.getString("pt_id").equals("pt_rating"))
    {
        NotificationManager nm = (NotificationManager) activity.getSystemService(Context.NOTIFICATION_SERVICE); 
        nm.cancel(payload.getInt("notificationId"));
    }
    if (payload.containsKey("pt_id")&& payload.getString("pt_id").equals("pt_product_display"))
    {
        NotificationManager nm = (NotificationManager) activity.getSystemService(Context.NOTIFICATION_SERVICE); 
        nm.cancel(payload.getInt("notificationId"));
    }
```
```kotlin
val payload = activity.intent?.extras
        if (payload?.containsKey("pt_id") == true && payload["pt_id"] =="pt_rating")
        {
            val nm = activity.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
            nm.cancel(payload["notificationId"] as Int)
        }
        if (payload?.containsKey("pt_id") == true && payload["pt_id"] =="pt_product_display")
        {
            val nm = activity.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
            nm.cancel(payload["notificationId"] as Int)
        }
```

## Android 12 Screenshots

Refer to the renditions of all the [Push Templates on Android 12 devices](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESANDROID12.md).

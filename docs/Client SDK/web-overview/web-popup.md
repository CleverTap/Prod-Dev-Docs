---
title: Web Pop-ups & Exit Intent
excerpt: Learn about the basic configurations for Web Pop-ups
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Web Push
      url: https://developer.clevertap.com/docs/web-push
---
# Web Pop-ups

A CleverTap web pop-up is a modal, alert-type message view displayed by CleverTap on your desktop or mobile website.


<Image src="https://files.readme.io/6126666-Web_Popup.png" alt="CleverTap Web Pop-ups" align="center" border={true} />


CleverTap web pop-ups come in two types:

* Simple web pop-up notifications: You can use these templates to create your notifications, including a box, banner, interstitial, and image only.
* Exit intent notifications: These notifications are displayed before the user exits your site. These are only available for desktops.

<Callout icon="📘" theme="info">
  ### Exit intent

  You can use exit intent to trigger Advanced Web Pop-ups when a user moves the pointer outside the browser window.

  Exit intent is supported for Advanced Web Pop-up Builder templates from Web SDK v2.6.4.
</Callout>

User clicks on a web notification can be silent, open a URL, or invoke a JavaScript function. Custom key-value pairs can also be passed to the invoked JavaScript function. You may also include a pre-set or custom image to be displayed in the notification.

## Advanced Web Popup Builder

The Advanced Web Popup Builder renders popups in a full-screen iframe managed by the CleverTap Web SDK.

<Callout icon="📘" theme="info">
  ### Dark mode behavior

  Advanced Builder popups are visually isolated from your site's color scheme. The SDK applies color-scheme: none to the iframe automatically, so system or browser dark-mode settings do not affect popup appearance. This behavior is available from Web SDK v2.6.2 and later.
</Callout>

## PIP (Picture-in-Picture)

The PIP template renders as a draggable, floating video or image popup that stays on screen while the user continues browsing. It is well-suited for video promotions, product demos, or any content that users might want to view alongside the rest of the page.

The PIP pop-up is implemented as a Web Custom Element `<ct-web-popup-pip>`. The SDK registers this element automatically — no additional setup is required from the developer.

<Callout icon="📘" theme="info">
  ### PIP Version

  PIP (Picture-in-Picture) is available from Web SDK v2.7.0 onwards.
</Callout>

### Anchor positions

The pop-up can be anchored to one of nine positions on the screen. Set the anchor in your campaign payload:

| Value          | Position               |
| -------------- | ---------------------- |
| `center`       | Center of the viewport |
| `top-left`     | Top-left corner        |
| `top`          | Top center             |
| `top-right`    | Top-right corner       |
| `left`         | Center-left            |
| `right`        | Center-right           |
| `bottom-left`  | Bottom-left corner     |
| `bottom`       | Bottom center          |
| `bottom-right` | Bottom-right corner    |

If no anchor is configured, the popup snaps to the nearest anchor based on its current on-screen position after the user releases the drag.

### Click actions

Click behavior is configured separately for desktop and mobile viewports in the campaign payload:

* **Desktop:** `display.pip.onClick`
* **Mobile (viewport ≤ 480px):** `display.mobile.pip.onClick` — falls back to `display.pip` if the mobile config is not set.

<Callout icon="🚧" theme="warn">
  ### Desktop vs. Mobile Breakpoint

  The SDK treats viewports wider than 480px (`window.innerWidth > 480`) as desktop. A viewport at exactly 480px is treated as mobile.
</Callout>

### Interactive controls

The PIP pop-up supports the following user-facing controls:

| Control           | Description                                                                                                 |
| ----------------- | ----------------------------------------------------------------------------------------------------------- |
| Play / Pause      | Toggle video playback                                                                                       |
| Mute / Unmute     | Toggle audio                                                                                                |
| Expand / Collapse | Expand the popup to a fullscreen overlay; collapse to return to the floating state                          |
| Drag              | Freely reposition the popup anywhere on screen (enabled via `controls.drag = true` in the campaign payload) |
| Open in new tab   | Open the popup's linked URL in a new browser tab                                                            |
| Close             | Dismiss the popup                                                                                           |

## Single-page applications

When clevertap.spa is set to `true`, active Web pop-ups are automatically dismissed when the user navigates to a new route. This applies to Box, Banner, and Image Only pop-ups.

If the user navigates to a route that matches the campaign conditions again, the pop-up can appear again.

For a full walkthrough of configuring SPA mode, including what the flag does, what happens without it, and version requirements, see [Single Page Applications](doc:web-quickstart-guide) in the Web SDK Quick Start Guide.

## Adding Close Buttons to Web Pop-ups

You can add a close button to your web pop-up by using the following:

### Box

```html
<div class="btn">CLOSE</div>
<script>
var btn = document.querySelector('.btn');
var wrapper = window.parent.document.getElementById('wizParDiv0');
btn.addEventListener('click', closePopUp);
function closePopUp() { 
  setTimeout(() => {
    wrapper.remove();
  }, 0); 
}
</script>
```

### Banner

```html
<div class="btn">CLOSE</div>
<script>
var btn = document.querySelector('.btn');
var wrapper = window.parent.document.getElementById('wizParDiv2');
btn.addEventListener('click', closePopUp);
function closePopUp() { 
  setTimeout(() => {
    wrapper.remove();
  }, 0);
}
</script>
```

### Interstitial

```html
<div class="btn">CLOSE</div>
<script>
var btn = document.querySelector('.btn');
var overlay = window.parent.document.getElementById('intentOpacityDiv');
var wrapper = window.parent.document.getElementById('intentPreview');
btn.addEventListener('click', closePopUp);
function closePopUp() { 
  setTimeout(() => {
    overlay.remove(); 
    wrapper.remove();
  }, 0); 
}
</script>
```

## Define a Custom Callback

Using the custom callback, you can control the look, feel, and location of the web pop-up which you create in your CleverTap dashboard.

You need to explicitly call clevertap.renderNotificationViewed(); and clevertap.renderNotificationClicked(); to ensure that notification views and clicks are tracked in your CleverTap dashboard.

To define the callback and raise the clicked and viewed events, you need to add the following snippet to the embed code:

```javascript
let customNotificationPayload = {
    msgId: string; // required
    pivotId?: string; // optional - String value containing an A/B testing campaign's variant name.
 }

clevertap.notificationCallback = function(msg){
      //raise the notification viewed and clicked events in the callback
      clevertap.renderNotificationViewed(customNotificationPayload);
      console.log(JSON.stringify(msg));            //your custom rendering implementation here
      var $button = jQuery("<button></button>");   //element on whose click you want to raise the notification clicked event
      $button.click(function(){
         clevertap.renderNotificationClicked(customNotificationPayload);
     });
};
```

The message will be in the following format:

* `msgId` consists of campaign ID and date stamp separated by an underscore.
* `kv` contains the custom key-value pairs.

```json Standard Web Pop-up Template
{
  "msgContent": {
     "html": "<Your HTML goes here/>",
     "type": 1,
     "templateType": "banner",  //templateType can also be box/interstitial
     "title": "Title goes here",
     "description": "Description goes here",
     "kv": {
         "key1": "value1",
         "key2": "value2"
      }
   },
   "msgId": "campaignId_date" //1630318544_20211004
}
```
```json Custom HTML Web Pop-up Template
{
       "msgContent":  {
            "html": "<Your HTML goes here/>",
            "type": 1,
            "templateType": "banner"  //templateType can also be box/interstitial
        },
        "msgId": "campaignId_date"  //1630318544_20211004
}
```

## Raising a Notification Clicked Event for Custom HTML Pop-ups

You can raise _Notification Clicked_ events for custom HTML-based Pop-ups for versions 1.1.0 and above.<br />To raise this event, follow the steps below:

1. Add a `<script>`tag to your web pop-up code, assign the `popupCallback` variable to a function, and save the notification object passed to it by the SDK.

```javascript
<script>
var notificationObj;
window.parent.clevertap.popupCallback = (notificationData) => {
    notificationObj = notificationData;
};
</script>
```

2. For `onclick` event handler functions, call the `raisePopupNotificationClicked` function with the notification object stored in Step 1.

```javascript
function submit_pressed() {
    window.parent.clevertap.raisePopupNotificationClicked(notificationObj)
}
```

## Custom HTML Click Tracking

The user must select the `Enable click tracking for Custom HTML` flag when creating a campaign from the CleverTap dashboard.

To track only selective buttons or anchor tags, set the  _Enable click tracking for Custom HTML_ flag<br />to `false` and manually add the `wzrk_c2a` attribute on the required buttons/anchor tags to track clicks. For example, `<button wzrk_c2a>Click Me</button>` will start tracking the _Click Me_ button.


<Image src="https://files.readme.io/f7899e9-click_tracking.png" alt="2412" align="center" border={true} />


# Dismiss Spam Control

The `dismissSpamControl` is a spam control flag that restricts repeat notifications to users after the user closes a Web Pop-up or Web Exit Intent notification.

### Behavior

* When `dismissSpamControl` = false:

  * This ensures that a campaign, once shown to a user, will not be shown again, until a new session begins and the user qualifies for it again.
  * This setting is ideal for most customers who expect one-time delivery of a campaign during a session, which does not have the same limits mentioned.
  * This is evaluated **at the campaign level**, not globally.

* When `dismissSpamControl` = true:
  * This overrides the above restriction and allows the same campaign to be shown repeatedly to a user, as long as the user satisfies the campaign conditions, even within the same session.
  * The campaign is triggered again even if the notification for the same campaign was previously closed by the user.
  * This is useful for specific or advanced use cases where campaign repetition is intentional.

<Callout icon="🚧" theme="warn">
  ### Dismiss Spam Control

  Starting from Web SDK version **2.0.0**, `dismissSpamControl` is set to true by default.

  If you want to control how often a user sees a notification (for example, show only once or show multiple times), you should configure Delivery Preference for that campaign. This gives you more precise control over user experience, independent of the `dismissSpamControl` behavior.
</Callout>

#### When to Use `dismissSpamControl` = false

Set this flag to false only if:

* You do not configure Delivery Preferences for a campaign.
* You want the campaign to show only once per session, and not reappear if dismissed, even if the user still qualifies.

In such cases, setting `dismissSpamControl` = false prevents users from being interrupted by the same message repeatedly.

---
title: mParticle Web Integration
excerpt: Learn how to integrate mParticle Web SDK with CleverTap.
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

You can integrate mParticle Web SDK with CleverTap to control the flow of information from the web SDK. You can send the following information to the CleverTap dashboard:

- Push User Information
- Push Events
- Integrate Push Notifications

# Prerequisites

The following are the prerequisites to perform this integration:

- A CleverTap account
- An mParticle account
- A functional website

# Integration

For CleverTap mParticle Web integration, the following are the major steps:

1. [Add Event Output](doc:mparticle-native-android-integration#add-event-output).
2. [Connect the Input Feed and Output Event on the mParticle dashboard](doc:mparticle-native-android-integration#connect-input-feed-and-output-event-on-mparticle-dashboard).
3. [Integrate the mParticle Web SDK](https://developer.clevertap.com/docs/mparticle-native-web-integration#integrate-the-segment-web-sdk).

## Add Event Output

To add an event output:

1. Navigate to _Setup_ > _Outputs_ from the mParticle dashboard.
2. Click **Add Event Output** and select _CleverTap_ from the dropdown list.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/df98cb8-AddEventOutput.png",
        "Select CleverTap from dropdown.png",
        2478
      ],
      "align": "center",
      "sizing": "90% ",
      "border": true
    }
  ]
}
[/block]

3. After selecting, _CleverTap Default Group_ is added under the _Events_ tab of the _Outputs_ page.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ed02d50-mParticleAddEventOutput.png",
        "CleverTap is added to Events tab.png",
        2478
      ],
      "align": "center",
      "sizing": "90% ",
      "border": true
    }
  ]
}
[/block]

4. Click the ![Add Event Configuration](https://files.readme.io/dfea20a-Plus_Icon.png) icon to add event configuration. The _Output: Event Configuration_ popup opens.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/33f6567-Output_Event_Configuration_popup.png",
        "Output Event Configuration popup.png",
        1116
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

5. Enter the following details and click **Save** to save the details:

[block:parameters]
{
  "data": {
    "h-0": "Field",
    "h-1": "Description",
    "0-0": "Configuration Name",
    "0-1": "Enter the name to identify your configuration uniquely.",
    "1-0": "Use the same settings for Development & Production",
    "1-1": "Select this option if you want to apply the same configuration settings for your production and development projects.  \nOR  \nSelect PROD or DEV to apply these configuration settings to the respective project.",
    "2-0": "Account ID, Account Passcode, and Account Token",
    "2-1": "Enter these details to authenticate your CleverTap account. Navigate to the _Settings_ > _Project_ page from the CleverTap dashboard to obtain these details.",
    "3-0": "Region",
    "3-1": "Enter the region of your CleverTap account. For more information, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region). "
  },
  "cols": 2,
  "rows": 4,
  "align": [
    "left",
    "left"
  ]
}
[/block]

The _Output Configuration Saved_ message is displayed at the top of the _Output_ page.

## Connect Input Feed and Output Event on mParticle Dashboard

To connect input feed and output event on the mParticle dashboard:

1. Navigate to the _Connections_ > _Connect_ page from the mParticle dashboard.
2. Select _Web_ from _Available Inputs_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6f21739-CleverTapmParticle.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

3. Click **Connect Output**. The _Connect Output_ pop-up opens.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9e3be9a-2023-03-15_12-56-21.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

4. Select _CleverTap_ and then select the _Configuration Name_ you want to connect.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8ee4281-mParticle_Web_Integration.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

5. Toggle the _Connection Status_ to forward data to the CleverTap dashboard.
6. Select _Customer ID_ from the _User ID_ dropdown list.
7. (Optional) Select _Min Platform Version_.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/12ee071-2023-03-15_13-00-59.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

8. Click **Add Connection** and click **Done**.
9. (Optional) Set up the forwarding rule for your output. It allows you to control the data you want to forward to the CleverTap dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a531ae2-2023-03-15_13-20-13.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

10. (Optional) Enter the percentage of the user base for which you want to event data to help control the cost.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3f9b0f9-2023-03-15_13-21-39.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "90% ",
      "border": true
    }
  ]
}
[/block]

11. Click **Save**.

## Integrate the mParticle Web SDK

To integrate mParticle Web SDK, add the following code snippet into the `<head>` tag of your website:

```javascript
<script type = "text/javascript" >
// Configures the SDK. Note the settings below for isDevelopmentMode and logLevel.
    window.mParticle = {
        config: {
            isDevelopmentMode: true,
            logLevel: verbose;
        },
    };
    (function(t) {
        window.mParticle = window.mParticle || {};
        window.mParticle.EventType = {
            Unknown: 0,
            Navigation: 1,
            Location: 2,
            Search: 3,
            Transaction: 4,
            UserContent: 5,
            UserPreference: 6,
            Social: 7,
            Other: 8
        };
        window.mParticle.eCommerce = {
            Cart: {}
        };
        window.mParticle.Identity = {};
        window.mParticle.config = window.mParticle.config || {};
        window.mParticle.config.rq = [];
        window.mParticle.config.snippetVersion = 2.3;
        window.mParticle.ready = function(t) {
            window.mParticle.config.rq.push(t)
        };
        var e = ["endSession", "logError", "logBaseEvent", "logEvent", "logForm", "logLink", "logPageView", "setSessionAttribute", "setAppName", "setAppVersion", "setOptOut", "setPosition", "startNewSession", "startTrackingLocation", "stopTrackingLocation"];
        var o = ["setCurrencyCode", "logCheckout"];
        var i = ["identify", "login", "logout", "modify"];
        e.forEach(function(t) {
            window.mParticle[t] = n(t)
        });
        o.forEach(function(t) {
            window.mParticle.eCommerce[t] = n(t, "eCommerce")
        });
        i.forEach(function(t) {
            window.mParticle.Identity[t] = n(t, "Identity")
        });

        function n(e, o) {
            return function() {
                if (o) {
                    e = o + "." + e
                }
                var t = Array.prototype.slice.call(arguments);
                t.unshift(e);
                window.mParticle.config.rq.push(t)
            }
        }
        var dpId, dpV, config = window.mParticle.config,
            env = config.isDevelopmentMode ? 1 : 0,
            dbUrl = "?env=" + env,
            dataPlan = window.mParticle.config.dataPlan;
        dataPlan && (dpId = dataPlan.planId, dpV = dataPlan.planVersion, dpId && (dpV && (dpV < 1 || dpV > 1e3) && (dpV = null), dbUrl += "&plan_id=" + dpId + (dpV ? "&plan_version=" + dpV : "")));
        var mp = document.createElement("script");
        mp.type = "text/javascript";
        mp.async = true;
        mp.src = ("https:" == document.location.protocol ? "https://jssdkcdns" : "http://jssdkcdn") + ".mparticle.com/js/v2/" + t + "/mparticle.js" + dbUrl;
        var c = document.getElementsByTagName("script")[0];
        c.parentNode.insertBefore(mp, c)
    })
    // Insert your API key below
    ("REPLACE WITH API KEY"); 
</script>
```

For more details on how to fetch API Key, refer to [Obtain API Key](https://docs.mparticle.com/developers/quickstart/web/create-input/#11-create-an-input) in mParticle documentation.

You can also integrate mParticle Web SDK using the package manager. 

`npm i @mparticle/web-sdk`

# Push User Information

After installing an app on a device, your user is assigned an anonymous profile. The first time the application identifies the user on the device, the anonymous history on the device is associated with the newly identified user. 

For more information about how user information is pushed to the mParticle dashboard, refer to [Track Users](https://docs.mparticle.com/developers/quickstart/web/track-users/) under mParticle documentation.

# Push Events

To learn how to push event data whenever you want to capture any user action in your application, refer to [Track Events](https://docs.mparticle.com/developers/quickstart/web/track-events/) under mParticle documentation.

# Integrate Push Notifications

For more information about integrating Push Notification on your website, refer to [Web Push Notifications](doc:web-push).
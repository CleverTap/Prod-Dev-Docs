---
title: Segment Web Integration
excerpt: Learn how to integrate Segment with CleverTap in Web.
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

Segment Web integration allows you to push your website data to CleverTap through Segment.\
With this integration, you can:

* Push User Profiles
* Push Events
* Integrate Push Notifications

# Prerequisites

The following are the prerequisites for this integration:

* A CleverTap account
* A Segment account

# Integration

For CleverTap Segment Web integration, the following are the major steps:

1. [Add Destination in Segment](https://developer.clevertap.com/docs/segment-web-integration#add-destination-in-segment)
2. [Integrate the Segment Web SDK](doc:segment-web-integration#integrate-the-segment-web-sdk)

## Add Destination in Segment

Destinations are the business tools or applications to which Segment forwards your data. Adding Destinations allows you to act on your data and learn more about your customers in real time.

To add CleverTap details in Segment:

1. Go to *Destinations* from the Segment dashboard.
2. Click *Add Destination* and select *CleverTap*.
3. Click **Configure CleverTap** from the *Destination Catalog/CleverTap* page.

<Image title="Configure CleverTap on Segment Dashboard" alt={2306} align="center" border={true} src="https://files.readme.io/8b60c1c-2023-02-01_13-15-02.png">
  Configure CleverTap on Segment Dashboard
</Image>

4. Add CleverTap *Account ID* and *Account Token*, which you can find on the CleverTap Dashboard under *Settings* page.
5. Select the appropriate region from the *Region* dropdown. To identify the region of your CleverTap account, refer to the following table:

| CleverTap Dashboard URL                                                                              | Region            |
| :--------------------------------------------------------------------------------------------------- | :---------------- |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/) | IN                |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/) | SG                |
| [https://us1.dashboard.clevertap.com/login.html#/](https://us1.dashboard.clevertap.com/login.html#/) | US                |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/) | None              |
| [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/) | Indonesia         |
| [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/) | Middle East (UAE) |

6. Turn on the toggle at the top of the *Settings* page to enable the destination.

<Image title="Enable Web Settings" alt={2354} align="center" border={true} src="https://files.readme.io/47d0d6d-2023-01-09_13-19-22.png">
  Enable Web Settings
</Image>

For more information about creating the source in the Segment dashboard, refer to [Create a Source](https://segment.com/docs/connections/sources/catalog/libraries/website/javascript/quickstart/#step-1-create-a-source-in-the-segment-app) under Segment documentation.

## Integrate the Segment Web SDK

To integrate Segment Web SDK, add the following code snippet into the `<head>` tag of your website:

```javascript JavaScript-Minified
<script>
    ! function() {
        var analytics = window.analytics = window.analytics || [];
        if (!analytics.initialize)
            if (analytics.invoked) window.console && console.error && console.error("Segment snippet included twice.");
            else {
                analytics.invoked = !0;
                analytics.methods = ["trackSubmit", "trackClick", "trackLink", "trackForm", "pageview", "identify", "reset", "group", "track", "ready", "alias", "debug", "page", "once", "off", "on", "addSourceMiddleware", "addIntegrationMiddleware", "setAnonymousId", "addDestinationMiddleware"];
                analytics.factory = function(e) {
                    return function() {
                        var t = Array.prototype.slice.call(arguments);
                        t.unshift(e);
                        analytics.push(t);
                        return analytics
                    }
                };
                for (var e = 0; e < analytics.methods.length; e++) {
                    var key = analytics.methods[e];
                    analytics[key] = analytics.factory(key)
                }
                analytics.load = function(key, e) {
                    var t = document.createElement("script");
                    t.type = "text/javascript";
                    t.async = !0;
                    t.src = "https://cdn.segment.com/analytics.js/v1/" + key + "/analytics.min.js";
                    var n = document.getElementsByTagName("script")[0];
                    n.parentNode.insertBefore(t, n);
                    analytics._loadOptions = e
                };
                analytics._writeKey = "YOUR_WRITE_KEY";
                analytics.SNIPPET_VERSION = "4.15.2";
                analytics.load("YOUR_WRITE_KEY");
                analytics.page();
            }
    }(); 
</script>
```
```javascript JavaScript-Non minified
<script type="text/javascript">
(function(){
  // Create a queue, but don't obliterate an existing one!
  var analytics = window.analytics = window.analytics || [];
  // If the real analytics.js is already on the page return.
  if (analytics.initialize) return;
  // If the snippet was invoked already show an error.
  if (analytics.invoked) {
    if (window.console && console.error) {
      console.error('Segment snippet included twice.');
    }
    return;
  }
  // Invoked flag, to make sure the snippet
  // is never invoked twice.
  analytics.invoked = true;
  // A list of the methods in Analytics.js to stub.
  analytics.methods = [
    'trackSubmit',
    'trackClick',
    'trackLink',
    'trackForm',
    'pageview',
    'identify',
    'reset',
    'group',
    'track',
    'ready',
    'alias',
    'debug',
    'page',
    'once',
    'off',
    'on',
    'addSourceMiddleware',
    'addIntegrationMiddleware',
    'setAnonymousId',
    'addDestinationMiddleware'
  ];
  // Define a factory to create stubs. These are placeholders
  // for methods in Analytics.js so that you never have to wait
  // for it to load to actually record data. The `method` is
  // stored as the first argument, so we can replay the data.
  analytics.factory = function(method){
    return function(){
      var args = Array.prototype.slice.call(arguments);
      args.unshift(method);
      analytics.push(args);
      return analytics;
    };
  };
  // For each of our methods, generate a queueing stub.
  for (var i = 0; i < analytics.methods.length; i++) {
    var key = analytics.methods[i];
    analytics[key] = analytics.factory(key);
  }
  // Define a method to load Analytics.js from our CDN,
  // and that will be sure to only ever load it once.
  analytics.load = function(key, options){
    // Create an async script element based on your key.
    var script = document.createElement('script');
    script.type = 'text/javascript';
    script.async = true;
    script.src = 'https://cdn.segment.com/analytics.js/v1/'
        + key + '/analytics.min.js';
    // Insert our script next to the first script element.
    var first = document.getElementsByTagName('script')[0];
    first.parentNode.insertBefore(script, first);
    analytics._loadOptions = options;
  };
  analytics._writeKey = 'YOUR_WRITE_KEY'
  // Add a version to keep track of what's in the wild.
  analytics.SNIPPET_VERSION = '4.15.2';
  // Load Analytics.js with your key, which will automatically
  // load the tools you've enabled for your account. Boosh!
  analytics.load("YOUR_WRITE_KEY");
  // Make the first page call to load the integrations. If
  // you'd like to manually name or tag the page, edit or
  // move this call however you'd like.
  analytics.page();
})();
</script>
```

| Parameter        | Description                                                                                                                                                      |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `YOUR_WRITE_KEY` | From the Segment dashboard, navigate to *Connections > Sources*. Select the source and then navigate to *Settings* > *API Keys* to find the *Write Key* details. |

> 📘 NPM Package
>
> You can add Segment to your project as an [NPM package](https://www.npmjs.com/package/@segment/snippet). For more information, refer to the instructions under [Adding Segment as an NPM Package](https://github.com/segmentio/analytics-next#-using-as-an-npm-package).

# Push User Information

To learn more about how user information is pushed to the Segment dashboard, refer to [Identify Users](https://segment.com/docs/connections/sources/catalog/libraries/website/javascript/quickstart/#step-3-identify-users) under Segment documentation.

> 📘 Update User Property to CleverTap
>
> To update any user property to CleverTap, pass the same user identity given during the user login/sign-up.

# Push Events

To learn how to push event data whenever you want to capture any user action on your website, refer to [Track Events](https://segment.com/docs/connections/sources/catalog/libraries/website/javascript/quickstart/#step-4-track-actions) under Segment documentation.

# Integrate Push Notifications

For more information about integrating Push Notification on your website, refer to [Web Push Notifications](doc:web-push).

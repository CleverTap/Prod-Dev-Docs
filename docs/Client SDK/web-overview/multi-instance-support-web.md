---
title: Multi-Instance Support Web
excerpt: Learn how to use multiple CleverTap accounts on a single website.
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  robots: index
---
## Overview

Starting with version 3.0.0 of the Web SDK, you can use multiple CleverTap accounts on a single website. This is useful in two common scenarios:

- **Multiple product areas on one site:** for example, a site with separate chat, map, and shopping sections can track each area with a different CleverTap account.
- **Region, brand, or business unit routing:** you can send data to a different CleverTap account depending on the region, brand, or business unit serving a given page.

<Callout icon="🚧" theme="warn">
  ### Instance limit

  The Web SDK supports a maximum of 5 instances: 1 default instance plus 4 additional instances. If you try to create a sixth instance, the SDK logs an error and does not create it.
</Callout>

## Prerequisites

Before you add additional instances, make sure you have:

- The CleverTap Web SDK, version 3.0.0 or later, already integrated on your site. For the base integration steps, refer to [Web SDK Quick Start Guide](https://developer.clevertap.com/docs/web-quickstart-guide).
- A separate CleverTap account (with its account ID, and optionally a passcode token) for each additional instance you plan to create.

## Default Instance Behavior

If you don't explicitly configure a default instance, the SDK automatically promotes the first instance you create to be the default. This happens whether you create that first instance with `createInstance()` (npm) or as the first entry in `clevertap.instances` (script tag).

The default instance behaves differently from additional instances:

| Behavior           | Default instance                                                                       | Additional instances                                |
| ------------------ | -------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Storage keys       | Unprefixed, for example `WZRK_G`, `WZRK_PR` (backward compatible with pre-3.0.0 sites) | Account-prefixed, for example `W9R-486-4W5Z:WZRK_G` |
| Global access      | Available at `window.clevertap`                                                        | Isolated from other instances                       |
| Global `$ct` state | Receives it                                                                            | Does not receive it                                 |

## Create Additional Instances

You can create additional instances two ways, depending on how you integrated the Web SDK: as a package (React, Vue, or plain npm) or as a script tag. The setup differs slightly depending on whether you want an explicit default instance or want the SDK to pick the default for you.

### Option A: Package Integration (React, Vue, npm)

With an explicit default instance, initialize the default instance first, then create each additional instance with `clevertap.CleverTap.createInstance()`:

```javascript
import clevertap from 'clevertap-web-sdk'

// Default instance
clevertap.init('YOUR_ACCOUNT_ID', 'YOUR_REGION', 'YOUR_TARGET_DOMAIN')
clevertap.privacy.push({ useIP: true })
clevertap.privacy.push({ optOut: false })
clevertap.setLogLevel(3)

// Additional instance
const ct2 = clevertap.CleverTap.createInstance({
  accountId: 'ADDITIONAL_CLEVERTAP_ACCOUNT_ID',
  region: 'eu1',
  targetDomain: 'clevertap-prod.com', // optional
  token: 'ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN' // optional
})
ct2.init()
ct2.privacy.push({ useIP: true })
ct2.setLogLevel(3)
ct2.enablePersonalization = true // default is false
Without an explicit default (first createInstance becomes default):

import clevertap from 'clevertap-web-sdk'

const ct = clevertap.CleverTap.createInstance({
  accountId: 'YOUR_ACCOUNT_ID',
  region: 'eu1'
})
ct.init()
ct.privacy.push({ useIP: true })
ct.setLogLevel(3)
```

If you don't call `clevertap.init()` yourself, the first call to `createInstance()` becomes the default instance instead.

<Callout icon="🚧" theme="warn">
  ### Call init() before using an instance

  Call `init()` on each additional instance before using any of its APIs. Configure privacy, log level, personalization, and encryption on the instance only after you create it.
</Callout>

### Option B: Script Tag Integration

The default instance keeps using the existing `clevertap.account.push()` and `clevertap.privacy.push()` pattern. You configure additional instances through a `clevertap.instances` array, where each entry is a self-contained configuration object. The SDK creates all instances as soon as the script loads.

With an explicit default instance:

```html
<script type="text/javascript">
  var clevertap = { event: [], profile: [], account: [], onUserLogin: [], notifications: [], privacy: [] };

  // Default instance
  clevertap.account.push({ "id": "YOUR_ACCOUNT_ID" }, "YOUR_REGION", "YOUR_TARGET_DOMAIN");
  clevertap.privacy.push({ optOut: false });
  clevertap.privacy.push({ useIP: false });

  // Additional instances
  clevertap.instances = [
    {
      accountId: "ADDITIONAL_CLEVERTAP_ACCOUNT_ID",
      region: "eu1",
      targetDomain: "clevertap-prod.com",
      token: "ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN",
      privacy: [{ optOut: false }, { useIP: true }]
    }
  ];

  (function () {
    var wzrk = document.createElement('script');
    wzrk.type = 'text/javascript';
    wzrk.async = true;
    wzrk.src = 'https://static.clevertap.com/js/clevertap.min.js';
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

If you don't add a default instance yourself, the first entry in `clevertap.instances` becomes the default:

```html
<script type="text/javascript">
  var clevertap = { event: [], profile: [], account: [], onUserLogin: [], notifications: [], privacy: [] };

  clevertap.instances = [
    {
      accountId: "YOUR_ACCOUNT_ID",
      region: "eu1",
      privacy: [{ optOut: false }, { useIP: true }]
    },
    {
      accountId: "ADDITIONAL_CLEVERTAP_ACCOUNT_ID",
      privacy: [{ optOut: false }]
    }
  ];

  (function () {
    var wzrk = document.createElement('script');
    wzrk.type = 'text/javascript';
    wzrk.async = true;
    wzrk.src = 'https://static.clevertap.com/js/clevertap.min.js';
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

Each entry in `clevertap.instances` supports the following fields.

| Field          | Required | Description                                                                          |
| -------------- | -------- | ------------------------------------------------------------------------------------ |
| `accountId`    | Yes      | The CleverTap account ID for this instance.                                          |
| `region`       | No       | The region code for this account.                                                    |
| `targetDomain` | No       | A custom target domain for this account.                                             |
| `token`        | No       | The passcode token for this account.                                                 |
| `privacy`      | No       | An array of privacy settings, such as `optOut` or `useIP`, applied to this instance. |

## Accessing Instances

The SDK exposes both the default and additional instances on `window`:

```javascript
window.clevertap.getAccountID()  // default account ID
window.clevertap.getCleverTapID()  // GUID for the default account

window.clevertap_YOUR_ACCOUNT_ID.getAccountID()  // additional account ID
window.clevertap_YOUR_ACCOUNT_ID.getCleverTapID()  // a different GUID
```

The SDK derives the `window` key from the instance's account ID by replacing every hyphen with an underscore. For example, account ID `YOUR-ACCOUNT-ID` becomes `window.clevertap_YOUR_ACCOUNT_ID`.

You can also retrieve the default instance programmatically instead of hardcoding `window.clevertap`:

```javascript
clevertap.getDefaultInstance()
clevertap.CleverTap.getDefaultInstance()
```

## Track Events with Additional Instances

### Package

Call `event.push()` on the additional instance itself, the same way you would on the default instance:

```javascript
clevertap.getDefaultInstance()
clevertap.CleverTap.getDefaultInstance()
Track Events with Additional Instances
Package
import clevertap from 'clevertap-web-sdk'

// Default instance
clevertap.init('YOUR_ACCOUNT_ID', 'YOUR_REGION', 'YOUR_TARGET_DOMAIN')

// Additional instance
const ct2 = clevertap.CleverTap.createInstance({
  accountId: 'ADDITIONAL_CLEVERTAP_ACCOUNT_ID',
  region: 'eu1',
  targetDomain: 'clevertap-prod.com', // optional
  token: 'ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN' // optional
})
ct2.init()

// Push an event
ct2.event.push('Event Name')

// Push an event with properties
ct2.event.push('Product viewed', {
  'Product Name': 'Casio Chronograph Watch',
  Category: 'Mens Accessories',
  Price: 59.99,
  Date: new Date()
})

// Push a Charged event
ct2.event.push('Charged', {
  Amount: 300,
  'Payment Mode': 'Credit card',
  'Charged ID': 24052013,
  Items: [
    {
      'Product category': 'books',
      'Book name': 'The Millionaire next door',
      Quantity: 1
    },
    {
      'Product category': 'books',
      'Book name': 'Achieving inner zen',
      Quantity: 1
    },
    {
      'Product category': 'books',
      'Book name': "Chuck it, let's do it",
      Quantity: 5
    }
  ]
})
Script Tag
```

### Script Tag

For the default instance, API calls work both before and after the SDK loads. Any call you queue before the script loads, such as `clevertap.event.push(...)`, waits in the queue and runs once the SDK initializes:

```html
<script>
  // Queued before the SDK loads, works for the default instance
  clevertap.event.push('Product Viewed');
</script>
```

For an additional instance, you must make API calls after the SDK has loaded, for example from a user interaction handler, because the instance doesn't exist until the script executes:

```html
<script>
  function onClickEventDefault() {
    clevertap.event.push('Purchase');
  }

  function onClickEventAdditional() {
    window.clevertap_W9R_486_4W5Z.event.push('Purchase');
  }
</script>
```

<Callout icon="📘" theme="info">
  ### Pre-load queuing only applies to the default instance

  Calling `clevertap.event.push(...)` before the script loads only works for the default instance. For additional instances, make all API calls after the SDK has loaded.
</Callout>

## Update Profiles with Additional Instances

### Package

Call `profile.push()` on the additional instance to update its user profile:

```javascript
import clevertap from 'clevertap-web-sdk'

// Default instance
clevertap.init('YOUR_ACCOUNT_ID', 'YOUR_REGION', 'YOUR_TARGET_DOMAIN')

// Additional instance
const ct2 = clevertap.CleverTap.createInstance({
  accountId: 'ADDITIONAL_CLEVERTAP_ACCOUNT_ID',
  region: 'eu1',
  targetDomain: 'clevertap-prod.com', // optional
  token: 'ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN' // optional
})
ct2.init()

// Push a simple profile update
ct2.profile.push({
  'Customer Type': 'Silver',
  'Preferred Language': 'English'
})

// Push a complex profile update
ct2.profile.push({
  Site: {
    Name: 'Jack Montana',
    Identity: 61026032,
    Email: 'jack@gmail.com',
    Phone: '+14155551234',
    Gender: 'M',
    DOB: new Date(),
    Photo: 'https://www.foobar.com/image.jpeg',
    'MSG-email': false,
    'MSG-push': true,
    'MSG-sms': false,
    MyStuff: ['bag', 'shoes']
  }
})
On User Login
Each instance maintains its own user identity and GUID. Use onUserLogin on the instance that should receive the identity update:

ct2.onUserLogin.push({
  Site: {
    Identity: '61026032',
    Email: 'jack@gmail.com'
  }
})
```

### Script Tag

<Callout icon="📘" theme="info">
  ### Inferred example, not from the source document

  The source document doesn't show a script tag example for profile updates. The call below follows the same `window.clevertap_<ACCOUNT_ID>.<queue>.push()` pattern already documented for events in [Track Events with Additional Instances](#track-events-with-additional-instances), applied to the `profile` queue. Verify against the SDK before publishing.
</Callout>

Call `profile.push()` on the additional instance's `window` object, after the SDK has loaded:

```html
<script>
  function onUpdateProfileAdditional() {
    window.clevertap_W9R_486_4W5Z.profile.push({
      'Customer Type': 'Silver',
      'Preferred Language': 'English'
    });
  }
</script>
```

### On User Login

#### Package

Each instance maintains its own user identity and GUID. Call `onUserLogin.push()` on whichever instance should receive the identity update:

```javascript
ct2.onUserLogin.push({
  Site: {
    Identity: '61026032',
    Email: 'jack@gmail.com'
  }
})
```

#### Script Tag

<Callout icon="📘" theme="info">
  ### Inferred example, not from the source document

  As with the profile update example above, the source document doesn't show a script tag example for `onUserLogin`. This follows the same `window.clevertap_<ACCOUNT_ID>.<queue>.push()` pattern applied to the `onUserLogin` queue. Verify against the SDK before publishing.
</Callout>

```html
<script>
  function onUserLoginAdditional() {
    window.clevertap_W9R_486_4W5Z.onUserLogin.push({
      Site: {
        Identity: '61026032',
        Email: 'jack@gmail.com'
      }
    });
  }
</script>
```

## Set Offline

You can take a CleverTap instance offline. By default, every instance is online (`false`).

While an instance is offline, the SDK still records events and queues them locally, but it doesn't send them to the server until you disable offline mode. When you call `setOffline(false)`, the SDK allows new events to send and immediately attempts to flush the queued events.

```javascript
ct2.setOffline(true)  // enable offline mode
ct2.setOffline(false)  // disable offline mode and flush queued events
```

<Callout icon="📘" theme="info">
  ### Offline mode is per instance

  Setting one instance offline does not affect any other instance.
</Callout>

## Encrypt PII Data

The Web SDK stores personally identifiable information (PII), such as email, identity, name, and phone number, in local storage. You can enable encryption so the SDK encrypts eligible PII keys before writing them to local storage. Encryption is disabled by default.

To enable local storage encryption for an additional instance:

```javascript
import clevertap from 'clevertap-web-sdk'

// Default instance
clevertap.init('YOUR_ACCOUNT_ID', 'YOUR_REGION', 'YOUR_TARGET_DOMAIN')

// Additional instance
const ct2 = clevertap.CleverTap.createInstance({
  accountId: 'ADDITIONAL_CLEVERTAP_ACCOUNT_ID',
  region: 'eu1',
  targetDomain: 'clevertap-prod.com', // optional
  token: 'ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN' // optional
})
ct2.init()
ct2.enableLocalStorageEncryption(true)

// Check whether encryption is enabled
ct2.isLocalStorageEncryptionEnabled() // true
```

<Callout icon="📘" theme="info">
  ### Encryption is per instance

  Enabling encryption on one instance does not enable it on any other instance. Repeat this step for every instance that needs it.
</Callout>

## FAQs

**How many CleverTap accounts can I use on one site?**<br />Up to 5, 1 default instance and up to 4 additional instances. Creating a 6th instance logs an error and the SDK does not create it.&#x20;

**Do I need to call&#x20;**`init()`**&#x20;on additional instances?**
Yes. Call `init()` on each additional instance before you call any of its other APIs.

**Does the default instance behave differently from additional instances?**
Yes. The default instance uses unprefixed storage keys and is available at `window.clevertap`. Additional instances use account-prefixed storage keys and are isolated from each other and from the default instance.

**Can I use pre-load event queuing with an additional instance?**
No. Pre-load queuing (pushing events before the SDK script loads) only works for the default instance.

## Additional Resources

- For the base Web SDK setup, refer to [Web SDK Quick Start Guide](https://developer.clevertap.com/docs/web-quickstart-guide).
- For the equivalent Android capability, refer to [Multi-Instance Support Android](https://developer.clevertap.com/docs/multi-instance-support-android).

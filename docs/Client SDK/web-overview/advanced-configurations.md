---
title: Advanced Configuration
excerpt: >-
  Learn more about advanced web configurations to help you customize your
  integration.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# GDPR

To comply with the General Data Protection Regulation (GDPR), you need to make specific changes to the installation of the CleverTap Web SDKs. These changes ensure that user data is handled in accordance with GDPR.

## Data Suppress

* This will ensure that the data from the device will not reach CleverTap's servers.
* By default, the optOut is set to False.
* If a device needs to be opted out of data collection, the suppression flag must be set at the JavaScript SDK level.

## IP Collection

* This will ensure that the CleverTap does not auto collect the device IP.
* By default, the useIP is set to False.
* If a customer wants to collect to the device IP, they need to explicitly set it to true.

```javascript Javascript
clevertap.privacy.push({optOut: true}); //call the flag to true, if the user of the device opts out of sharing their data

clevertap.privacy.push({useIP: false}); //call the flag to true, if the user agrees to share their IP data
```

For more information, visit the [Web Quick Start Guide](https://developer.clevertap.com/docs/web-quickstart-guide).

# Encryption of PII Data

To protect client information, we ensure that all sensitive data is encrypted. From CleverTap Web SDK v1.13.0 onwards, you can enable encryption for Personally Identifiable Information (PII) data such as Email, Identity, Name, and Phone.

We have implemented a separate encryption mechanism for local storage data to ensure its security. The encryption key will be derived from the customer's account ID and used to encrypt and decrypt local storage data.

The following keys contain PII data and will be encrypted.

| Keys    | Description                                                    |
| :------ | :------------------------------------------------------------- |
| WZRK_K  | Contains information about the latest logged-in user.          |
| WZRK_X  | Stores a cache of all users logged in from a specific machine. |
| WZRK_PR | Contains user profile details.                                 |

We support an opt-in encryption mechanism, allowing users to choose whether to enable encryption for their local storage data. To enable encryption, use the following method:

`clevertap.enableLocalStorageEncryption(true)`

# Encrypt Data in Transit

CleverTap Web SDK always transmits request bodies in plain text (can be compressed if required).To comply with security and compliance requirements, CleverTap provides *Encryption in Transit * to protect sensitive customer data as it moves between the browser and CleverTap.

When this feature is enabled, all SDK requests are encrypted before being sent.

## Enable Encryption in Transit

You can enable encryption in transit using either script tags or the NPM package.

### Using Script Tags

Set the `enableEncryptionInTransit` flag to `true` in the global `clevertap` object.

```html
<script type="text/javascript">
  var clevertap = {
    event: [],
    profile: [],
    account: [],
    onUserLogin: [],
    notifications: [],
    privacy: [],
    enableEncryptionInTransit: true // New option
  };

  // Replace CLEVERTAP_ACCOUNT_ID with the actual Account ID
  // from Dashboard > Settings
  clevertap.account.push({ id: "CLEVERTAP_ACCOUNT_ID" });

  // Set to true if the user opts out of sharing their data
  clevertap.privacy.push({ optOut: false });

  // Set to true if the user agrees to share their IP data
  clevertap.privacy.push({ useIP: false });

  (function () {
    var wzrk = document.createElement('script');
    wzrk.type = 'text/javascript';
    wzrk.async = true;
    wzrk.src = 'https://d2r1yp2w7bby2u.cloudfront.net/js/clevertap.min.js';
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

### Using NPM

Pass `enableEncryptionInTransit: true` as part of the initialization options.

```javascript
import clevertap from 'clevertap-web-sdk';

// Set to true if the user opts out of sharing their data
clevertap.privacy.push({ optOut: false });

// Set to true if the user agrees to share their IP data
clevertap.privacy.push({ useIP: false });

// Replace the values with those applicable to your account
clevertap.init(
  'ACCOUNT_ID',
  'REGION',
  'TARGET_DOMAIN',
  'TOKEN',
  { enableEncryptionInTransit: true }
);
```

<Callout icon="📘" theme="info">
  Note

  Check that this feature is enabled for your account. If encryption in transit is not enabled at the billing level, SDK requests will continue to be sent in plain text even if `enableEncryptionInTransit` is set to `true`.
</Callout>

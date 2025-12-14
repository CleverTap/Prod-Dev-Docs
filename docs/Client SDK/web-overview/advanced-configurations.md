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

| Keys     | Description                                                    |
| :------- | :------------------------------------------------------------- |
| WZRK\_K  | Contains information about the latest logged-in user.          |
| WZRK\_X  | Stores a cache of all users logged in from a specific machine. |
| WZRK\_PR | Contains user profile details.                                 |

We support an opt-in encryption mechanism, allowing users to choose whether to enable encryption for their local storage data. To enable encryption, use the following method:

`clevertap.enableLocalStorageEncryption(true)`

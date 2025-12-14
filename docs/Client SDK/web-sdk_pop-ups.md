---
title: Web SDK_Pop Ups
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
## Overview

This section demonstrates how to send web pop-ups and push notifications. Before following the steps in this section, be sure to complete the [Web SDK Quick Start Guide](doc:web-quickstart-guide) which shows how to set up CleverTap’s JavaScript library on your website, track events, create user profiles, and engage users by showing them a notification.

# Web Pop-ups

A CleverTap web pop-up is a modal, alert-type message view displayed by CleverTap on your desktop or mobile website.

![445](https://files.readme.io/7cb150e-image1.png "image1.png")

CleverTap web pop-ups come in two types:

* Simple web pop-up notifications: You can use these templates to create your notifications including a box notification, a banner notification, and an interstitial notification.
* Exit intent notifications: These notifications are displayed before the user exits your site. These are only available for desktop. 

User clicks on a web notification can be silent, open a URL, or invoke a JavaScript function. Custom key-value pairs can also be passed to the invoked JavaScript function. You may also include a pre-set or custom image to be displayed in the notification.

> 🚧 Click Tracking
>
> At the moment, we do not support tracking clicks on custom web pop-ups.

## Adding Close Buttons to Web Pop-ups

You can add a close button to your web pop-up by using the following: 

### Box

```html
<div class="btn">CLOSE</div>
<script>
var btn = document.querySelector('.btn');
var wrapper = window.parent.document.getElementById('wizParDiv0');
btn.addEventListener('click', closePopUp);
function closePopUp()
{ wrapper.remove(); }
</script>
```

### Banner

```html
<div class="btn">CLOSE</div>
<script>
var btn = document.querySelector('.btn');
var wrapper = window.parent.document.getElementById('wizParDiv2');
btn.addEventListener('click', closePopUp);
function closePopUp() { wrapper.remove(); }
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
function closePopUp()
{ overlay.remove(); wrapper.remove(); }
</script>
```

# Defining a Custom Rendering Listener

Using this custom callback, you can control the look, feel, and location of the web pop-up which you create in your CleverTap dashboard.

You need to explicitly call clevertap.raiseNotificationViewed(); and clevertap.raiseNotificationClicked(); to ensure that notification views and clicks are tracked in your CleverTap dashboard.

To define the callback and raise the clicked and viewed events, you need to add the following snippet to the embed code:

```javascript
clevertap.notificationCallback = function(msg){
      //raise the notification viewed and clicked events in the callback
      clevertap.raiseNotificationViewed();
      console.log(JSON.stringify(msg));//your custom rendering implementation here
      var $button = jQuery("<button></button>");//element on whose click you want to raise the notification clicked event
      $button.click(function(){
         clevertap.raiseNotificationClicked();
      });
};
```

The message will be in the format below:

* `msgId` contains the campaign ID and the date stamp, so you can programmatically decide whether to show the notification or not. 
* `kv` contains the custom key value pairs.

```json
{
    "msgContent": {
        "title": "hello title!",
        "description": “hello message!"
    },
    "msgId": "1439796272_20160219",
    "kv": {
        "key1":"value1",
        "key2":"value2"
    }
}
```

# Web Push

At the moment, web push notifications are supported for Chrome version 50 for desktop and Android, and only work on HTTPS sites.

## Chrome Web Push

### Add the CleverTap JavaScript Library

Integrate the CleverTap JavaScript library by following the [Web Quick Start Guide](doc:web-quickstart-guide).

### Integrate FCM and CleverTap

FCM has deprecated its existing web push protocol after July 2019 and moved to the VAPID protocol.

If your server key and sender ID were generated before July 2019, the legacy protocol will continue to work. However, if they were generated after July 2019, only the VAPID protocol will be supported.

We recommend using the VAPID protocol due to increased security.

#### Legacy Protocol

Get your sender ID and server key, then use the following steps: 

1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *Chrome Web Push* section, add your server key.
3. Click **Save**.

<Image title="New_Web_Push_Add_server_key.png" alt={2128} className="border" border={true} src="https://files.readme.io/6341ba8-New_Web_Push_Add_server_key.png" />

#### VAPID Protocol

Get your public and private key pair from Firebase with the following steps: 

1. In the Firebase console, navigate to your *Project* > *Project Settings* > *Cloud Messaging* > *Web Configuration*.
2. Click **Generate key pair**.

<Image title="New-New_API_Firebase.png" alt={2880} className="border" border={true} src="https://files.readme.io/cd27210-New-New_API_Firebase.png" />

3. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
4. Select *Use VAPID spec*.
5. Under the *Chrome Web Push* section, add your generated public and private key pairs.
6. Click **Save**.

<Image title="New-24_VAPID_specs.png" alt={2118} className="border" border={true} src="https://files.readme.io/44b40a6-New-24_VAPID_specs.png" />

> 📘 Re-subscription of Users
>
> After you move from legacy to the VAPID protocol, we recommend re-subscribing the users with legacy tokens to move them to VAPID. In this case, the permission pop-up will not re-appear. The subscription will happen in the browser's background silently.
>
> To help you in the migration process, we also support the legacy protocol simultaneously, even though VAPID is enabled. This ensures web push delivery across all your subscribed users.

## Firefox Web Push

This section covers the Firefox web push.

> 📘 Mobile Devices
>
> At the moment, we do not support web push on mobile devices.

### Add the CleverTap JavaScript Library

Integrate the CleverTap JavaScript library by following the [Web Quickstart Guide](doc:web-quickstart-guide).

### Generate VAPID keys

The VAPID keys can be generated using the following methods:

1. Using npm package name web-push (recommended).
2. You can also use VAPID keys generated for Chrome web push in FCM account. For more information, refer to [VAPID Protocol](https://developer.clevertap.com/docs/web#section-vapid-protocol). The same VAPID keys will be valid for Chrome as well as Firefox web push. 

### Paste VAPID Keys in CleverTap

Get your VAPID keys, then use the following steps: 

1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *Firefox Web Push* section, add your generated VAPID public and private key pairs. 
3. Click **Save**.

<Image title="25.png" alt={1448} className="border" border={true} src="https://files.readme.io/d09620c-25.png" />

## Add the Service Worker File

This section is applicable for Chrome and Firefox browsers. Push notifications on browser use a special script called a service worker that listens for web pushes. Since the execution of these scripts is managed by the browser in the background, users can receive push notifications from your website even when they are not on your site. To get started, download the service worker file we have provided and copy it in your document root of your website in order to make it publicly accessible.

Download the service worker file [from this JavaScript code](http://static.clevertap.com/docs/clevertap_sw.js). As mentioned above, you will have to host this file in your document root.

### Integration with Existing Service Worker File

Import CleverTap's service worker at the start of your existing service worker. Add the following code snippet:

```javascript
importScripts('https://s3-eu-west-1.amazonaws.com/static.wizrocket.com/js/sw_webpush.js');// remove CleverTap server worker from your root folder
```

## Specify the Sender ID in manifest.json File

This section is applicable for Chrome and Firefox browsers.

> 📘 Legacy Protocol
>
> This step is only required if you are using the legacy protocol.

To register the service worker and subscribe for push notifications, you need to specify your sender ID inside a website manifest file.

If your website already has a manifest, add the following property to it. Your sender ID is your FCM project number (12-digit number)—not your API key.

```json
{
  "gcm_sender_id": "YOUR_SENDER_ID"
}
```

If you do not have a manifest file yet, create a new file called manifest.json and add it in the document root of your website. Ensure that you specify the "name" and "gcm\_sender\_id" parameters where "name" is the name of your brand (e.g. Acme Fashion).

Here is a sample manifest.json file:

```json
{
  "name": "Acme Fashion | Latest in fashion world",
  "gcm_sender_id": "1234567890" // replace with actual value
}
```

Then in all your HTML files, include the snippet below in the header:

```html
<head>
  <link rel="manifest" href="/manifest.json">
</head>
```

## Safari Web Push

This section covers the Safari web push.

> 📘 Mobile Devices
>
> At the moment, we do not support web push on mobile devices.

### Dashboard Setting

Enable Safari web push with the following steps: 

1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *Safari Web Push* section, select the toggle *Safari Web Push*.
3. Provide your credentials.
4. Click **Save**.

![1448](https://files.readme.io/f18fdb3-26.png "26.png")

### Set Safari

To setup a Safari web push certificate, follow the steps below:

In OS X v10.9 and later, you can dispatch Safari push notifications from your webserver to OS X users by using the Apple Push Notification service (APNs). This is different from local notifications because push notifications can reach your users even if your website or Safari browser is not open.

![1352](https://files.readme.io/adc9e41-Safari_Push_Workflow.png "Safari_Push_Workflow.png")

To integrate push notifications on your website, you must first present an interface that allows the user to opt-in to receive notifications. If the user consents, Safari requests for its credentials on your website in the form of a file called a push package. The push package also contains notification assets used throughout OS X and data used to communicate to a web service that you must configure. If the push package is valid, you receive a unique identifier also known as device token for the user. The user receives the notification when you send the combination of this device token and your message, or payload to APNs.

This guide helps you with process management and maintaining a coherent way of getting your service implemented. 

The following steps cover how to implement your service:

1. Generate your [push certificate](https://developer.clevertap.com/docs/web#section-generate-push-certificate). You will require the Apple WWDRCA certificate provided by Apple from your developer portal to send notifications. Another certificate is needed when actually sending notifications and also, while creating the push package. 

While not mentioned in Apple’s official documentation, you will need two certificates:

* For registering app with APNs.
* Establishing a token-based connection to APNs.

2. Configure the [raw push package folder](https://developer.clevertap.com/docs/web#section-create-push-package) and then, use the companion [PHP file](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/NotificationProgrammingGuideForWebsites/CompanionFile.zip) to zip up your final push package. We will also check for any issues while creating the push package in the debugging section.

Your push package is a folder of icons and configurations used by each Mac device that subscribes to your notification service.

3. Create a [webserver](https://developer.clevertap.com/docs/web#section-webserver) to verify your push package. 

4. Run the [front end Javascript](https://developer.clevertap.com/docs/web#section-client-side-code) to subscribe to your notifications and get the device token that is returned. Granting client access to your notifications is easy using Javascript. If a valid push package is returned, access is granted and a device token is given in a JSON response.

### Generate Push Certificate

You must register in the [*Certificates, Identifiers & Profiles*](https://developer.apple.com/account/ios/certificate) section of your developer account to send push notifications. You must have an Apple developer license to register. 

* Identifier: This is your unique reverse-domain string, such as *web.com.example.domain* (the string must start with web). This is also known as the *Website Push ID*.
* Website push ID description: This is the name used throughout the provisioning portal to refer to your website. Use it for your own benefit to label your website push IDs into a more human-readable format.

<Image title="Safari_Push_register_an_identifier.png" alt={1600} className="border" border={true} src="https://files.readme.io/55b7310-Safari_Push_register_an_identifier.png" />

![1600](https://files.readme.io/99c4bb8-Safari_Push_register_a_website_push_ID.png "Safari_Push_register_a_website_push_ID.png")

After the push ID is registered, create a production certificate of that push ID by clicking **Create Certificate** under *Edit your Identifier Configuration*.

<Image title="Safari_Push_Production_Certificate.png" alt={1387} className="border" border={true} src="https://files.readme.io/0905d6f-Safari_Push_Production_Certificate.png" />

If you already have the certificate signing request, then upload the file from your local machine. If not, refer to [Create a certificate signing request](https://help.apple.com/developer-account/#/devbfa00fef7) in the Apple documentation. 

![1600](https://files.readme.io/5281311-Safari_Push_Signing_Request.png "Safari_Push_Signing_Request.png")

You can download the push certificate after the certificate signing request is uploaded:

![1600](https://files.readme.io/bd3e73d-New-Safari_Push_certificate_download.png "New-Safari_Push_certificate_download.png")

Double-click the downloaded certificate and add it to your keychain:

1. Navigate to your *Keychains* > *My Certificates*.
2. Select your certificate and private key.

![990](https://files.readme.io/73d77a3-Safari_Push_Keychain_Select.png "Safari_Push_Keychain_Select.png")

3. Export both as .p12. 
4. Add a password. This step is very important.

![824](https://files.readme.io/e0ac566-Safari_Push_Signing_Request_password.png "Safari_Push_Signing_Request_password.png")

You will require one more certificate named *Apple Worldwide Developer Relations Certification Authority*. You can download it from the [Apple Certification Authority](https://developer.apple.com/certificationauthority/AppleWWDRCA.cer).

5. Add the AWDRCA certificate to the keychain and export it as .pem file.
6. Sign the push package with both the web push certificate and the intermediate certificate.

![1600](https://files.readme.io/8af960b-Safari_add_AWDRCA_certificate_.png "Safari_add_AWDRCA certificate .png")

### Create a Push Package

When a user is asked for permission to receive push notifications, Safari asks your webserver for a package. The package contains data that is used by the notification UI, such as your website name and icon, as well as a cryptographic signature. The signature verifies that your notification has not been intercepted by a man-in-the-middle attack and that it is indeed coming from a trusted source.

Below is the folder structure you must create:

```text
pushpackage/
    icon.iconset/
        icon_16x16.png
        icon_16x16@2x.png
        icon_32x32.png
        icon_32x32@2x.png
        icon_128x128.png
        icon_128x128@2x.png
     manifest.json
     signature
     website.json
```

#### Website JSON

The website JSON dictionary named website.json contains metadata used by Safari and Notification Center to present UI to the user and to communicate with your web service.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Key
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        websiteName
      </td>

      <td>
        The website name. This is the heading used in the Notification Center.
      </td>
    </tr>

    <tr>
      <td>
        websitePushID
      </td>

      <td>
        The website push ID as specified in your developer account.
      </td>
    </tr>

    <tr>
      <td>
        allowedDomains
      </td>

      <td>
        An array of websites that are allowed to request permission from the user.
      </td>
    </tr>

    <tr>
      <td>
        urlFormatString
      </td>

      <td>
        The URL to go to when the notification is clicked. Use %@ as a placeholder for arguments you fill in when delivering your notification. This URL must use the http or https scheme; otherwise, it is invalid.
      </td>
    </tr>

    <tr>
      <td>
        authenticationToken
      </td>

      <td>
        A string that helps you identify the user. It is included in later requests to your web service. This string must be 16 characters or greater.
      </td>
    </tr>

    <tr>
      <td>
        webServiceURL
      </td>

      <td>
        The location used to make requests to your web service. The trailing slash should be omitted. This should be https.
      </td>
    </tr>
  </tbody>
</Table>

Following is a sample valid website.json file:

```json
{
    "websiteName": "Bay Airlines",
    "websitePushID": "web.com.example.domain",
    "allowedDomains": ["http://domain.example.com"],
    "urlFormatString": "http://domain.example.com/%@/?flight=%@",
    "authenticationToken": "19f8d7a6e9fb8a7f6d9330dabe",
    "webServiceURL": "https://example.com/push"
}
```

#### Iconset

The iconset is a directory called `icon.iconset` that contains PNG images in varying sizes. The images in your iconset populate the icons displayed to the user in the permission prompt, Notification Center, and the notification itself. Since your icon is static, it is unnecessary to include it in every push notification. Instead, your icons are downloaded once from your server and stored on the user’s computer.

The icons should be valid as per the Apple guidelines. Use [https://tinypng.com/](https://tinypng.com/) to compress your icons to the required size.

#### Manifest

A manifest is a JSON dictionary named manifest.json that contains an entry for each file where the local file path is the entry’s key and a dictionary object is the entry’s value. This dictionary contains the hashType and hashValue which is the file’s SHA512 checksum.

#### Signature

The signature is a PKCS #7 detached signature of the manifest file. Sign the manifest file with the private key associated with your web push certificate that you obtained while registering with Apple.

We will use a PHP file named createPushPackage.php to generate the zip file for the pushpackage folder above. The folder to create a push package is present in the solution under the folder named “safari\_push\_package\_creator”.

1. Copy and paste the following files in one folder:

* .p12 certificate we created above by registering.
* PHP file.
* .pem file created using AWDRCA certificate.
* Create a tmp folder, then pushPackage inside it.

2. Create one pushpackage.raw folder and paste the following files in it:

* icon.iconset folder and all the icons mentioned above in it.
* Website.json as per the guidelines mentioned above.

3. Open the PHP file and modify it as per the following instructions:

* Change $certificate\_path to path of .p12 certificate.
* Change $certificate\_password to password of .p12 certificate.
* Replace "\<path to .pem>" under create\_signature with path to .pem file.
* Change $package\_dir to path to tmp/pushPackage directory.

 The AWDRCA.pem file is mandatory after 2014 for security purposes.  

4. After the above steps are done, run the PHP file in the terminal:\
   php createPushPackage.php

If run successfully under the tmp folder, a pushPackage.zip file will be created. You can place this pushPackage.zip in your web service directory which will send the pushPackage.zip to the client when a request for Safari push notifications will be requested.

> 🚧 Considerations
>
> Some things to consider:
>
> * All the files must be present.
> * Use the zipping method used in the PHP file attached as the order of the files is important.
> * Icons must be valid.
> * Use the valid certificates. If they expire, renew them.

### Code Overview

This overview covers the [client-side](https://developer.clevertap.com/docs/web#section-client-side-code) and web service code that return the pushPackage.zip file to the client (Safari browser).

#### Webserver

When Safari requests permission to display push notifications, an HTTPS request for credentials is sent to your webserver. Configure a RESTful HTTPS web service on your webserver to respond to these requests. The web service can be hosted on any server or domain. The webserver allows the request for push notification and sends a push package (application/zip format) to Safari. This push package is verified by Safari and it sends a push token to the CleverTap SDK. 

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Fragment
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        webServiceURL
      </td>

      <td>
        The URL to your web service; this is the same as the URL parameter in `window.safari.pushNotification.requestPermission()`. It must start with https.
      </td>
    </tr>

    <tr>
      <td>
        version
      </td>

      <td>
        The version of the API. Currently, “v1”.
      </td>
    </tr>

    <tr>
      <td>
        deviceToken
      </td>

      <td>
        The unique identifier for the user on the device.
      </td>
    </tr>

    <tr>
      <td>
        websitePushID
      </td>

      <td>
        The website push ID.
      </td>
    </tr>
  </tbody>
</Table>

Configure endpoints in a RESTful https web service on your server to respond:

1. Download your website package.\
   This POST request contains the following information:

The URL format is webServiceURL/version/pushPackages/websitePushID\
For example: [https://yourwebsite.com/v1/pushPackages/web.com.yourwebsite](https://yourwebsite.com/v1/pushPackages/web.com.yourwebsite)

2. Register or update the device permission policy.\
   This POST request contains the following information:

The URL format is webServiceURL/version/devices/deviceToken/registrations/websitePushID\
For example: [https://yourwebsite.com/v1/devices/deviceToken/registrations/web.com.yourwebsite](https://yourwebsite.com/v1/devices/deviceToken/registrations/web.com.yourwebsite)

3. Forget the device permission policy.\
   This DELETE request contains the following information:

The URL format is webServiceURL/version/devices/deviceToken/registrations/websitePushID\
For example: [https://yourwebsite.com/v1/devices/deviceToken/registrations/web.com.yourwebsite](https://yourwebsite.com/v1/devices/deviceToken/registrations/web.com.yourwebsite)

4. Log errors.\
   If an error occurs, a POST request is sent to the following URL:

The URL format is webServiceURL/version/log\
For example: [https://yourwebsite.com/v1/log](https://yourwebsite.com/v1/log) 

#### Client Side Code

To request permission to send the user push notifications, call the `window.safari.pushNotification.requestPermission()` method.

Requesting permission is an asynchronous call\
`window.safari.pushNotification.requestPermission(url, websitePushID, userInfo, callback);`

A description of each argument is as follows:

* URL: The URL of the web service which must start with https. The webserver and the website requesting permission can have different domains. 
* websitePushID: The website push ID as specified in your developer account for the generated certificate. It follows the convention on an Apple account and must start with web. For example, web.yourname.safari is a Safari certificate ID.
* userInfo: An object required to pass to the server. Include any data in this object that helps you identify the user requesting permission.
* callback: A callback function which is invoked on completion.

Following is a sample code:

```javascript
//For Safari
function requestPermission() {
    if ('safari' in window && 'pushNotification' in window.safari) {

        var permissionData =
            window.safari.pushNotification.permission('web.pjay.push');
        window.safari.pushNotification.requestPermission(
            'https://localhost:8443/Push_notification_war_exploded',
            'web.pjay.push', {}, function(subscription) {
                console.log(subscription);
                if(subscription.permission === 'granted') {
                    subscriptionInfoSafari = subscription;
                    safariToken = subscription.deviceToken;
                    sending(safariToken);
                    // updateSubscriptionOnServer(subscription);
                }
                else if(subscription.permission === 'denied') {
                    // TODO:
                }
            });

    } else {
        alert("Push notifications not supported.");
    }
}
```

#### Troubleshooting

If there is an issue with downloading the push package or delivery of push notifications, the logging endpoint on your web service as described in the *Log errors* (section above) is contacted with an error message and error description. 

The following table lists the possible errors and steps you can take to fix them:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Error Message
      </th>

      <th>
        Resolution
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        AuthenticationToken must be at least 16 characters
      </td>

      <td>
        The `authenticationToken` key in your website.json file must be 16 characters or greater.
      </td>
    </tr>

    <tr>
      <td>
        Downloading push notification package failed
      </td>

      <td>
        The Safari push package could not be retrieved from the specified location.
      </td>
    </tr>

    <tr>
      <td>
        Extracting push notification package failed
      </td>

      <td>
        Check that your push package is zipped correctly.
      </td>
    </tr>

    <tr>
      <td>
        Missing file in push notification package
      </td>

      <td>
        Check that your push package contains all of the files specified in [building the push package](https://developer.clevertap.com/docs/web#section-create-push-package).
      </td>
    </tr>

    <tr>
      <td>
        Missing image in push notification package
      </td>

      <td>
        Check that your push package contains all of the files specified in the [iconset folder](https://developer.clevertap.com/docs/web#section-create-push-package).
      </td>
    </tr>

    <tr>
      <td>
        Missing key in website.json
      </td>

      <td>
        Check that your website.json file has all of the keys listed in the Website JSON dictionary.
      </td>
    </tr>

    <tr>
      <td>
        Serialization of JSON in website.json failed
      </td>

      <td>
        The website.json file in your push package is not valid JSON.
      </td>
    </tr>

    <tr>
      <td>
        Signature verification of push package failed
      </td>

      <td>
        The manifest was not signed correctly or was signed using an invalid certificate.
      </td>
    </tr>

    <tr>
      <td>
        Unable to create a notification bundle for push notification package
      </td>

      <td>
        The extracted push package could not be saved to the user’s disk.
      </td>
    </tr>

    <tr>
      <td>
        Unable to generate ICNS file for push notification package
      </td>

      <td>
        Your iconset may have malformed PNGs.
      </td>
    </tr>

    <tr>
      <td>
        Unable to parse webServiceURL
      </td>

      <td>
        Check that the value for `webServiceURL` in your website.json file is a valid URL.
      </td>
    </tr>

    <tr>
      <td>
        Unable to save push notification package
      </td>

      <td>
        The push package could not be saved to the user’s disk.
      </td>
    </tr>

    <tr>
      <td>
        urlFormatString must have http or https scheme
      </td>

      <td>
        Make sure that the value for `urlFormatString` in your website.json file starts with http or https.
      </td>
    </tr>

    <tr>
      <td>
        Verifying hashes in manifest.json failed
      </td>

      <td>
        The SHA512 checksums specified in your manifest.json file do not compute to their actual values.
      </td>
    </tr>

    <tr>
      <td>
        Web Service API URL must be https
      </td>

      <td>
        Make sure that the URL at your push package endpoint starts with https.
      </td>
    </tr>

    <tr>
      <td>
        webServiceURL must be equal to URL in call to requestPermission
      </td>

      <td>
        Cross-check that the web service URL in your JavaScript call matches the Web service URL in your website.json file.
      </td>
    </tr>

    <tr>
      <td>
        websitePushID must be equal to identifier in call to requestPermission
      </td>

      <td>
        Cross-check that the identifier in your JavaScript call matches the identifier in your website.json file.
      </td>
    </tr>

    <tr>
      <td>
        x cannot be used as a format string for URLs
      </td>

      <td>
        The URL created by inserting the notification payload’s url-args values into the placeholders specified in the push package’s `urlFormatString` is not valid, or there are a different number of values than placeholders.
      </td>
    </tr>
  </tbody>
</Table>

## Requesting User Permissions

This section is applicable for all browsers such as, Chrome, Firefox, and Safari. Now that you have a service worker and manifest set up, it is time to start asking users if they want to subscribe to web pushes. For flexibility, CleverTap provides you with the ability to ask for notifications based on context and user actions.

As a general rule, we highly recommend asking for permission from users only after they have spent some time on your website and completed action of significance. Do not ask new users to subscribe to notifications as soon as they land on your site.

For example, after a user has completed a transaction, you may call the function below:

```javascript
clevertap.notifications.push({
  "apnsWebPushId":"<apple web push id>", //only for safari browser
  "apnsWebPushServiceUrl":"<safari package service url>",//only for safari browser
  "titleText":'Would you like to receive Push Notifications?',
  "bodyText":'We promise to only send you relevant content and give you updates on your transactions',
  "okButtonText":'Sign me up!',
  "rejectButtonText":'No thanks',
  "okButtonColor":'#f28046'});
```

The general format for creating the pop-up is shown below:

```javascript
clevertap.notifications.push({
    "titleText":popupTitleText,
    "bodyText":popupBodyText,
    "okButtonText":okButtonText,
    "rejectButtonText":rejectButtonText,
    "okButtonColor":okButtonColorInHex});
```

The custom service worker format for notification permission pop-up is shown below:

```javascript
clevertap.notifications.push({
  
	"apnsWebPushId”: ‘<apple web push id>‘,
	"apnsWebPushServiceUrl”: ‘<safari package service url>’, 
   	"“titleText”:‘Would you like to receive Push Notifications?’,
   	"bodyText”:‘We promise to only send you relevant content and give you updates on your transactions’,
   	"okButtonText”:‘Sign me up!’,
   	"rejectButtonText”:‘No thanks’,
   	"okButtonColor”:‘#F28046’,
   	"askAgainTimeInSeconds”:5,
  	"serviceWorkerPath”: “/foo/my_sw.js” // path to your custom service worker file
  });
```

If a user blocks the native Chrome permissions pop-up, the user is lost forever unless the user clears their browser settings; however, if you are using the CleverTap pop-up, you get the advantage of asking the same user again for permission after a defined period.

### Permission Frequency

To ensure that users are not asked for permission too frequently, our API ensures that calling clevertap.notifications.push does nothing if the user has rejected the dialog less than one week ago. Once a week has passed by, calling the function will display the dialog as expected. This eliminates the need for you to implement a separate check before calling the function. 

To set a separate frequency, send "askAgainTimeInSeconds":time\_in\_seconds along with the above object. 

To skip the CleverTap pop-up, send "skipDialog":true along with the object above.

## Migrating from Other Services

This section is applicable for all browsers such as, Chrome, Firefox, and Safari. If you are migrating from a different web push notifications provider, you may be concerned about asking subscribed users to enable notifications a second time.

> 📘 Migrating Subscribed Users
>
> If a user has already enabled notifications on your host domain, calling clevertap.notifications.push will not display the pop-up for that particular user and the registration will automatically be migrated to our new implementation.

You can use the custom service file below which should only be used during the migration phase. Once the token migration of most of your users is complete, you can replace it with the [parent service worker](https://developer.clevertap.com/docs/web#section-add-the-service-worker-file). 

```javascript
importScripts('https://s3-eu-west-1.amazonaws.com/static.wizrocket.com/js/sw_webpush_only_registration.js');
```

## Debugging

This section is applicable for all browsers such as, Chrome, Firefox, and Safari. Error messages and warnings are logged to the JS console of the browser.

For verbose logging, enable verbose logging of all communication with the CleverTap servers by setting the WZRK\_D variable in sessionStorage. In the developer console of your browser type, use: sessionStorage['WZRK_D'] = '';

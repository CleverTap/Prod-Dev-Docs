---
title: Web
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
This guide demonstrates how to send web pop-ups and push notifications.

Before following this guide, be sure to complete the [Web Quick Start Guide](doc:web-quickstart-guide) which shows you how to setup CleverTap’s JavaScript library on your website, track events, create user profiles, and engage users by showing them a notification.

# Web Pop-ups

A CleverTap web pop-up is a modal, alert-type message view displayed by CleverTap on your desktop or mobile website.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7cb150e-image1.png",
        "image1.png",
        445,
        182,
        "#546475"
      ]
    }
  ]
}
[/block]
CleverTap web pop-ups come in two types:
  * Simple web pop-up notifications: You can use these templates to create your notifications including a box notification, a banner notification, and an interstitial notification.
  * Exit intent notifications: These notifications are displayed before the user exits your site. These are only available for desktop. 

User clicks on a web notification can be silent, open a URL, or invoke a JavaScript function. Custom key value pairs can also be passed to the invoked JavaScript function. You may also include a pre-set or custom image to be displayed in the notification.
[block:callout]
{
  "type": "warning",
  "body": "At the moment, we do not support tracking clicks on custom web pop-ups.",
  "title": "Click Tracking"
}
[/block]
##Adding Close Buttons to Web Pop-ups
You can add a close button to your web pop-up by using the following: 

###Box

[block:code]
{
  "codes": [
    {
      "code": "<div class=\"btn\">CLOSE</div>\n<script>\nvar btn = document.querySelector('.btn');\nvar wrapper = window.parent.document.getElementById('wizParDiv0');\nbtn.addEventListener('click', closePopUp);\nfunction closePopUp()\n{ wrapper.remove(); }\n</script>\n\n",
      "language": "html"
    }
  ]
}
[/block]
###Banner

[block:code]
{
  "codes": [
    {
      "code": "<div class=\"btn\">CLOSE</div>\n<script>\nvar btn = document.querySelector('.btn');\nvar wrapper = window.parent.document.getElementById('wizParDiv2');\nbtn.addEventListener('click', closePopUp);\nfunction closePopUp() { wrapper.remove(); }\n</script>\n\n",
      "language": "html"
    }
  ]
}
[/block]
###Interstitial
[block:code]
{
  "codes": [
    {
      "code": "<div class=\"btn\">CLOSE</div>\n<script>\nvar btn = document.querySelector('.btn');\nvar overlay = window.parent.document.getElementById('intentOpacityDiv');\nvar wrapper = window.parent.document.getElementById('intentPreview');\nbtn.addEventListener('click', closePopUp);\nfunction closePopUp()\n{ overlay.remove(); wrapper.remove(); }\n</script> \n\n",
      "language": "html"
    }
  ]
}
[/block]
#Defining a Custom Rendering Listener
Using this custom callback, you can control the look, feel, and location of the web pop-up which you create in your CleverTap dashboard.

You need to explicitly call clevertap.raiseNotificationViewed(); and clevertap.raiseNotificationClicked(); to ensure that notification views and clicks are tracked in your CleverTap dashboard.

To define the callback and raise the clicked and viewed events, you need to add the following snippet to the embed code:
[block:code]
{
  "codes": [
    {
      "code": "clevertap.notificationCallback = function(msg){\n      //raise the notification viewed and clicked events in the callback\n      clevertap.raiseNotificationViewed();\n      console.log(JSON.stringify(msg));//your custom rendering implementation here\n      var $button = jQuery(\"<button></button>\");//element on whose click you want to raise the notification clicked event\n      $button.click(function(){\n         clevertap.raiseNotificationClicked();\n      });\n};",
      "language": "javascript"
    }
  ]
}
[/block]
The message will be in the format below:
* `msgId` contains the campaign ID and the date stamp, so you can programmatically decide whether to show the notification or not. 
* `kv` contains the custom key value pairs.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"msgContent\": {\n        \"title\": \"hello title!\",\n        \"description\": “hello message!\"\n    },\n    \"msgId\": \"1439796272_20160219\",\n    \"kv\": {\n        \"key1\":\"value1\",\n        \"key2\":\"value2\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
# Web Push
At the moment, web push notifications are supported for Chrome version 50 for desktop and Android, and only work on HTTPS sites.

##Chrome Web Push

###Add the CleverTap JavaScript Library
Integrate the CleverTap JavaScript library by following the [Web Quick Start Guide](doc:web-quickstart-guide).

###Integrate FCM and CleverTap
FCM has deprecated its existing web push protocol after July 2019 and moved to the VAPID protocol.

If your server key and sender ID were generated before July 2019, the legacy protocol will continue to work. However, if they were generated after July 2019, only the VAPID protocol will be supported.

We recommend using the VAPID protocol due to increased security.

####Legacy Protocol

Get your sender ID and server key, then use the following steps: 
1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *Chrome Web Push* section, add your server key.
3. Click **Save**.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6341ba8-New_Web_Push_Add_server_key.png",
        "New_Web_Push_Add_server_key.png",
        2128,
        1674,
        "#fbfbfc"
      ],
      "border": true
    }
  ]
}
[/block]
####VAPID Protocol

Get your public and private key pair from Firebase with the following steps: 
1. In the Firebase console, navigate to your *Project* > *Project Settings* > *Cloud Messaging* > *Web Configuration*.
2. Click **Generate key pair**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cd27210-New-New_API_Firebase.png",
        "New-New_API_Firebase.png",
        2880,
        1800,
        "#d4d8dd"
      ],
      "border": true
    }
  ]
}
[/block]
3. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
4. Select *Use VAPID spec*.
5. Under the *Chrome Web Push* section, add your generated public and private key pairs.
6. Click **Save**.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/44b40a6-New-24_VAPID_specs.png",
        "New-24_VAPID_specs.png",
        2118,
        1674,
        "#fafafb"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Re-subscription of Users",
  "body": "After you move from legacy to the VAPID protocol, we recommend re-subscribing the users with legacy tokens to move them to VAPID. In this case, the permission pop-up will not re-appear. The subscription will happen in the browser's background silently.\n\nTo help you in the migration process, we also support the legacy protocol simultaneously, even though VAPID is enabled. This ensures web push delivery across all your subscribed users."
}
[/block]
##Firefox Web Push
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "At the moment, we do not support web push on mobile devices."
}
[/block]

###Add the CleverTap JavaScript Library
Integrate the CleverTap JavaScript library by following the [Web Quickstart Guide](doc:web-quickstart-guide).

###Generate VAPID keys
The VAPID keys can be generated using the following methods:
1. Using npm package name web-push (recommended).
2. You can also use VAPID keys generated for Chrome web push in FCM account. For more information, refer to [VAPID Protocol](https://developer.clevertap.com/docs/web#section-vapid-protocol). The same VAPID keys will be valid for Chrome as well as Firefox web push. 

###Paste VAPID Keys in CleverTap
Get your VAPID keys, then use the following steps: 

1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *Firefox Web Push* section, add your generated VAPID public and private key pairs. 
3. Click **Save**.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d09620c-25.png",
        "25.png",
        1448,
        826,
        "#f9fafb"
      ],
      "border": true
    }
  ]
}
[/block]
##Add the Service Worker File
This section is applicable for Chrome and Firefox browsers. Push notifications on browser use a special script called a service worker that listens for web pushes. Since the execution of these scripts is managed by the browser in the background, users can receive push notifications from your website even when they are not on your site. To get started, download the service worker file we have provided and copy it in your document root of your website in order to make it publicly accessible.

Download the service worker file [from this JavaScript code](http://static.clevertap.com/docs/clevertap_sw.js). As mentioned above, you will have to host this file in your document root.

###Integration with Existing Service Worker File
Import CleverTap's service worker at the start of your existing service worker. Add the following code snippet:
[block:code]
{
  "codes": [
    {
      "code": "importScripts('https://s3-eu-west-1.amazonaws.com/static.wizrocket.com/js/sw_webpush.js');// remove CleverTap server worker from your root folder",
      "language": "javascript"
    }
  ]
}
[/block]
##Specify the Sender ID in manifest.json File
This section is applicable for Chrome and Firefox browsers.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "This step is only required if you are using the legacy protocol."
}
[/block]
To register the service worker and subscribe for push notifications, you need to specify your sender ID inside a website manifest file.

If your website already has a manifest, add the following property to it. Your sender ID is your FCM project number (12-digit number)—not your API key.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"gcm_sender_id\": \"YOUR_SENDER_ID\"\n}",
      "language": "json"
    }
  ]
}
[/block]
If you do not have a manifest file yet, create a new file called manifest.json and add it in the document root of your website. Ensure that you specify the "name" and "gcm_sender_id" parameters where "name" is the name of your brand (e.g. Acme Fashion).

Here is a sample manifest.json file:
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"Acme Fashion | Latest in fashion world\",\n  \"gcm_sender_id\": \"1234567890\" // replace with actual value\n}",
      "language": "json"
    }
  ]
}
[/block]
Then in all your HTML files, include the snippet below in the header:
[block:code]
{
  "codes": [
    {
      "code": "<head>\n  <link rel=\"manifest\" href=\"/manifest.json\">\n</head>",
      "language": "html"
    }
  ]
}
[/block]
##Safari Web Push

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "At the moment, we do not support web push on mobile devices."
}
[/block]
###Dashboard Setting

Enable Safari web push with the following steps: 
1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *Safari Web Push* section, select the toggle *Safari Web Push*.
3. Provide your credentials.
4. Click **Save**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f18fdb3-26.png",
        "26.png",
        1448,
        988,
        "#fafafc"
      ]
    }
  ]
}
[/block]
###Set Safari

To setup a Safari web push certificate, follow the steps below:

In OS X v10.9 and later, you can dispatch Safari push notifications from your webserver to OS X users by using the Apple Push Notification service (APNs). This is different from local notifications because push notifications can reach your users even if your website or Safari browser is not open.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/adc9e41-Safari_Push_Workflow.png",
        "Safari_Push_Workflow.png",
        1352,
        526,
        "#e5eff2"
      ]
    }
  ]
}
[/block]
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

###Generate Push Certificate

You must register in the [*Certificates, Identifiers & Profiles*](https://developer.apple.com/account/ios/certificate) section of your developer account to send push notifications. You must have an Apple developer license to register. 

  * Identifier: This is your unique reverse-domain string, such as *web.com.example.domain* (the string must start with web). This is also known as the *Website Push ID*.
  * Website push ID description: This is the name used throughout the provisioning portal to refer to your website. Use it for your own benefit to label your website push IDs into a more human-readable format.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/55b7310-Safari_Push_register_an_identifier.png",
        "Safari_Push_register_an_identifier.png",
        1600,
        1016,
        "#f6f6f6"
      ],
      "border": true,
      "caption": ""
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/99c4bb8-Safari_Push_register_a_website_push_ID.png",
        "Safari_Push_register_a_website_push_ID.png",
        1600,
        862,
        "#f9f9fa"
      ],
      "caption": ""
    }
  ]
}
[/block]
After the push ID is registered, create a production certificate of that push ID by clicking **Create Certificate** under *Edit your Identifier Configuration*.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0905d6f-Safari_Push_Production_Certificate.png",
        "Safari_Push_Production_Certificate.png",
        1387,
        609,
        "#f9f9f9"
      ],
      "border": true
    }
  ]
}
[/block]
If you already have the certificate signing request, then upload the file from your local machine. If not, refer to [Create a certificate signing request](https://help.apple.com/developer-account/#/devbfa00fef7) in the Apple documentation. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5281311-Safari_Push_Signing_Request.png",
        "Safari_Push_Signing_Request.png",
        1600,
        862,
        "#f9f9fa"
      ]
    }
  ]
}
[/block]
You can download the push certificate after the certificate signing request is uploaded:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bd3e73d-New-Safari_Push_certificate_download.png",
        "New-Safari_Push_certificate_download.png",
        1600,
        724,
        "#f9fafa"
      ]
    }
  ]
}
[/block]
Double-click the downloaded certificate and add it to your keychain:
1. Navigate to your *Keychains* > *My Certificates*.
2. Select your certificate and private key.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/73d77a3-Safari_Push_Keychain_Select.png",
        "Safari_Push_Keychain_Select.png",
        990,
        666,
        "#282a32"
      ]
    }
  ]
}
[/block]

3. Export both as .p12. 
4. Add a password. This step is very important.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e0ac566-Safari_Push_Signing_Request_password.png",
        "Safari_Push_Signing_Request_password.png",
        824,
        589,
        "#404041"
      ]
    }
  ]
}
[/block]
You will require one more certificate named *Apple Worldwide Developer Relations Certification Authority*. You can download it from the [Apple Certification Authority](https://developer.apple.com/certificationauthority/AppleWWDRCA.cer).

5. Add the AWDRCA certificate to the keychain and export it as .pem file.
6. Sign the push package with both the web push certificate and the intermediate certificate.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8af960b-Safari_add_AWDRCA_certificate_.png",
        "Safari_add_AWDRCA certificate .png",
        1600,
        1069,
        "#2f3135"
      ]
    }
  ]
}
[/block]
###Create a Push Package
When a user is asked for permission to receive push notifications, Safari asks your webserver for a package. The package contains data that is used by the notification UI, such as your website name and icon, as well as a cryptographic signature. The signature verifies that your notification has not been intercepted by a man-in-the-middle attack and that it is indeed coming from a trusted source.

Below is the folder structure you must create:

[block:code]
{
  "codes": [
    {
      "code": "pushpackage/\n    icon.iconset/\n        icon_16x16.png\n        icon_16x16@2x.png\n        icon_32x32.png\n        icon_32x32@2x.png\n        icon_128x128.png\n        icon_128x128@2x.png\n     manifest.json\n     signature\n     website.json\n     \n     ",
      "language": "text"
    }
  ]
}
[/block]
####Website JSON
The website JSON dictionary named website.json contains metadata used by Safari and Notification Center to present UI to the user and to communicate with your web service.
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Description",
    "0-0": "websiteName",
    "0-1": "The website name. This is the heading used in the Notification Center.",
    "1-0": "websitePushID",
    "1-1": "The website push ID as specified in your developer account.",
    "2-0": "allowedDomains",
    "2-1": "An array of websites that are allowed to request permission from the user.",
    "3-0": "urlFormatString",
    "3-1": "The URL to go to when the notification is clicked. Use %@ as a placeholder for arguments you fill in when delivering your notification. This URL must use the http or https scheme; otherwise, it is invalid.",
    "4-0": "authenticationToken",
    "4-1": "A string that helps you identify the user. It is included in later requests to your web service. This string must be 16 characters or greater.",
    "5-0": "webServiceURL",
    "5-1": "The location used to make requests to your web service. The trailing slash should be omitted. This should be https."
  },
  "cols": 2,
  "rows": 6
}
[/block]
Following is a sample valid website.json file:
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"websiteName\": \"Bay Airlines\",\n    \"websitePushID\": \"web.com.example.domain\",\n    \"allowedDomains\": [\"http://domain.example.com\"],\n    \"urlFormatString\": \"http://domain.example.com/%@/?flight=%@\",\n    \"authenticationToken\": \"19f8d7a6e9fb8a7f6d9330dabe\",\n    \"webServiceURL\": \"https://example.com/push\"\n}\n\n\n\n",
      "language": "json"
    }
  ]
}
[/block]
####Iconset
The iconset is a directory called `icon.iconset` that contains PNG images in varying sizes. The images in your iconset populate the icons displayed to the user in the permission prompt, Notification Center, and the notification itself. Since your icon is static, it is unnecessary to include it in every push notification. Instead, your icons are downloaded once from your server and stored on the user’s computer.

The icons should be valid as per the Apple guidelines. Use https://tinypng.com/ to compress your icons to the required size.

####Manifest
A manifest is a JSON dictionary named manifest.json that contains an entry for each file where the local file path is the entry’s key and a dictionary object is the entry’s value. This dictionary contains the hashType and hashValue which is the file’s SHA512 checksum.


####Signature
The signature is a PKCS #7 detached signature of the manifest file. Sign the manifest file with the private key associated with your web push certificate that you obtained while registering with Apple.

We will use a PHP file named createPushPackage.php to generate the zip file for the pushpackage folder above. The folder to create a push package is present in the solution under the folder named “safari_push_package_creator”.

1. Copy and paste the following files in one folder:
  * .p12 certificate we created above by registering.
  * PHP file.
  * .pem file created using AWDRCA certificate.
  * Create a tmp folder, then pushPackage inside it.

2. Create one pushpackage.raw folder and paste the following files in it:
  * icon.iconset folder and all the icons mentioned above in it.
  * Website.json as per the guidelines mentioned above.

3. Open the PHP file and modify it as per the following instructions:
  * Change $certificate_path to path of .p12 certificate.
  * Change $certificate_password to password of .p12 certificate.
  * Replace "<path to .pem>" under create_signature with path to .pem file.
  * Change $package_dir to path to tmp/pushPackage directory.

 The AWDRCA.pem file is mandatory after 2014 for security purposes.  

4. After the above steps are done, run the PHP file in the terminal:
php createPushPackage.php

If run successfully under the tmp folder, a pushPackage.zip file will be created. You can place this pushPackage.zip in your web service directory which will send the pushPackage.zip to the client when a request for Safari push notifications will be requested.

[block:callout]
{
  "type": "warning",
  "title": "Considerations",
  "body": "* All the files must be present.\n* Use the zipping method used in the PHP file attached as the order of the files is important.\n* Icons must be valid.\n* Use the valid certificates. If they expire, renew them."
}
[/block]

###Code Overview
This overview covers the [client-side](https://developer.clevertap.com/docs/web#section-client-side-code) and web service code that return the pushPackage.zip file to the client (Safari browser).

####Webserver

When Safari requests permission to display push notifications, an HTTPS request for credentials is sent to your webserver. Configure a RESTful HTTPS web service on your webserver to respond to these requests. The web service can be hosted on any server or domain. The webserver allows the request for push notification and sends a push package (application/zip format) to Safari. This push package is verified by Safari and it sends a push token to the CleverTap SDK. 

[block:parameters]
{
  "data": {
    "h-0": "Fragment",
    "h-1": "Description",
    "0-0": "webServiceURL",
    "0-1": "The URL to your web service; this is the same as the URL parameter in `window.safari.pushNotification.requestPermission()`. It must start with https.",
    "1-0": "version",
    "1-1": "The version of the API. Currently, “v1”.",
    "2-0": "deviceToken",
    "2-1": "The unique identifier for the user on the device.",
    "3-0": "websitePushID",
    "3-1": "The website push ID."
  },
  "cols": 2,
  "rows": 4
}
[/block]
Configure endpoints in a RESTful https web service on your server to respond:

1. Download your website package.
This POST request contains the following information:

The URL format is webServiceURL/version/pushPackages/websitePushID
For example: https://yourwebsite.com/v1/pushPackages/web.com.yourwebsite

2. Register or update the device permission policy.
This POST request contains the following information:

The URL format is webServiceURL/version/devices/deviceToken/registrations/websitePushID
For example: https://yourwebsite.com/v1/devices/deviceToken/registrations/web.com.yourwebsite

3. Forget the device permission policy.
This DELETE request contains the following information:

The URL format is webServiceURL/version/devices/deviceToken/registrations/websitePushID
For example: https://yourwebsite.com/v1/devices/deviceToken/registrations/web.com.yourwebsite

4. Log errors.
If an error occurs, a POST request is sent to the following URL:

The URL format is webServiceURL/version/log
For example: https://yourwebsite.com/v1/log 

####Client Side Code
To request permission to send the user push notifications, call the `window.safari.pushNotification.requestPermission()` method.

Requesting permission is an asynchronous call 
`window.safari.pushNotification.requestPermission(url, websitePushID, userInfo, callback);`

A description of each argument is as follows:
* URL: The URL of the web service which must start with https. The webserver and the website requesting permission can have different domains. 
* websitePushID: The website push ID as specified in your developer account for the generated certificate. It follows the convention on an Apple account and must start with web. For example, web.yourname.safari is a Safari certificate ID.
* userInfo: An object required to pass to the server. Include any data in this object that helps you identify the user requesting permission.
* callback: A callback function which is invoked on completion.

Following is a sample code:
[block:code]
{
  "codes": [
    {
      "code": "//For Safari\nfunction requestPermission() {\n    if ('safari' in window && 'pushNotification' in window.safari) {\n\n        var permissionData =\n            window.safari.pushNotification.permission('web.pjay.push');\n        window.safari.pushNotification.requestPermission(\n            'https://localhost:8443/Push_notification_war_exploded',\n            'web.pjay.push', {}, function(subscription) {\n                console.log(subscription);\n                if(subscription.permission === 'granted') {\n                    subscriptionInfoSafari = subscription;\n                    safariToken = subscription.deviceToken;\n                    sending(safariToken);\n                    // updateSubscriptionOnServer(subscription);\n                }\n                else if(subscription.permission === 'denied') {\n                    // TODO:\n                }\n            });\n\n    } else {\n        alert(\"Push notifications not supported.\");\n    }\n}",
      "language": "javascript"
    }
  ]
}
[/block]
####Troubleshooting

If there is an issue with downloading the push package or delivery of push notifications, the logging endpoint on your web service as described in the *Log errors* (section above) is contacted with an error message and error description. 

The following table lists the possible errors and steps you can take to fix them:
[block:parameters]
{
  "data": {
    "h-0": "Error Message",
    "h-1": "Resolution",
    "0-0": "AuthenticationToken must be at least 16 characters",
    "0-1": "The `authenticationToken` key in your website.json file must be 16 characters or greater.",
    "1-0": "Downloading push notification package failed",
    "1-1": "The Safari push package could not be retrieved from the specified location.",
    "2-0": "Extracting push notification package failed",
    "2-1": "Check that your push package is zipped correctly.",
    "3-0": "Missing file in push notification package",
    "3-1": "Check that your push package contains all of the files specified in [building the push package](https://developer.clevertap.com/docs/web#section-create-push-package).",
    "4-0": "Missing image in push notification package",
    "4-1": "Check that your push package contains all of the files specified in the [iconset folder](https://developer.clevertap.com/docs/web#section-create-push-package).",
    "5-0": "Missing key in website.json",
    "5-1": "Check that your website.json file has all of the keys listed in the Website JSON dictionary.",
    "6-0": "Serialization of JSON in website.json failed",
    "6-1": "The website.json file in your push package is not valid JSON.",
    "7-0": "Signature verification of push package failed",
    "7-1": "The manifest was not signed correctly or was signed using an invalid certificate.",
    "8-0": "Unable to create a notification bundle for push notification package",
    "8-1": "The extracted push package could not be saved to the user’s disk.",
    "9-0": "Unable to generate ICNS file for push notification package",
    "9-1": "Your iconset may have malformed PNGs.",
    "10-0": "Unable to parse webServiceURL",
    "10-1": "Check that the value for `webServiceURL` in your website.json file is a valid URL.",
    "11-0": "Unable to save push notification package",
    "11-1": "The push package could not be saved to the user’s disk.",
    "12-0": "urlFormatString must have http or https scheme",
    "12-1": "Make sure that the value for `urlFormatString` in your website.json file starts with http or https.",
    "13-0": "Verifying hashes in manifest.json failed",
    "13-1": "The SHA512 checksums specified in your manifest.json file do not compute to their actual values.",
    "14-0": "Web Service API URL must be https",
    "14-1": "Make sure that the URL at your push package endpoint starts with https.",
    "15-0": "webServiceURL must be equal to URL in call to requestPermission",
    "15-1": "Cross-check that the web service URL in your JavaScript call matches the Web service URL in your website.json file.",
    "16-0": "websitePushID must be equal to identifier in call to requestPermission",
    "16-1": "Cross-check that the identifier in your JavaScript call matches the identifier in your website.json file.",
    "17-0": "x cannot be used as a format string for URLs",
    "17-1": "The URL created by inserting the notification payload’s url-args values into the placeholders specified in the push package’s `urlFormatString` is not valid, or there are a different number of values than placeholders."
  },
  "cols": 2,
  "rows": 18
}
[/block]
##Requesting User Permissions
This section is applicable for all browsers such as, Chrome, Firefox, and Safari. Now that you have a service worker and manifest set up, it is time to start asking users if they want to subscribe to web pushes. For flexibility, CleverTap provides you with the ability to ask for notifications based on context and user actions.

As a general rule, we highly recommend asking for permission from users only after they have spent some time on your website and completed action of significance. Do not ask new users to subscribe to notifications as soon as they land on your site.

For example, after a user has completed a transaction, you may call the function below:
[block:code]
{
  "codes": [
    {
      "code": "clevertap.notifications.push({\n  \"apnsWebPushId\":\"<apple web push id>\", //only for safari browser\n  \"apnsWebPushServiceUrl\":\"<safari package service url>\",//only for safari browser\n  \"titleText\":'Would you like to receive Push Notifications?',\n  \"bodyText\":'We promise to only send you relevant content and give you updates on your transactions',\n  \"okButtonText\":'Sign me up!',\n  \"rejectButtonText\":'No thanks',\n  \"okButtonColor\":'#f28046'});\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
The general format for creating the pop-up is shown below:
[block:code]
{
  "codes": [
    {
      "code": "clevertap.notifications.push({\n    \"titleText\":popupTitleText,\n    \"bodyText\":popupBodyText,\n    \"okButtonText\":okButtonText,\n    \"rejectButtonText\":rejectButtonText,\n    \"okButtonColor\":okButtonColorInHex});\n",
      "language": "javascript"
    }
  ]
}
[/block]
The custom service worker format for notification permission pop-up is shown below:
[block:code]
{
  "codes": [
    {
      "code": "clevertap.notifications.push({\n  \n\t\"apnsWebPushId”: ‘<apple web push id>‘,\n\t\"apnsWebPushServiceUrl”: ‘<safari package service url>’, \n   \t\"“titleText”:‘Would you like to receive Push Notifications?’,\n   \t\"bodyText”:‘We promise to only send you relevant content and give you updates on your transactions’,\n   \t\"okButtonText”:‘Sign me up!’,\n   \t\"rejectButtonText”:‘No thanks’,\n   \t\"okButtonColor”:‘#F28046’,\n   \t\"askAgainTimeInSeconds”:5,\n  \t\"serviceWorkerPath”: “/foo/my_sw.js” // path to your custom service worker file\n  });",
      "language": "javascript"
    }
  ]
}
[/block]
If a user blocks the native Chrome permissions pop-up, the user is lost forever unless the user clears their browser settings; however, if you are using the CleverTap pop-up, you get the advantage of asking the same user again for permission after a defined period.

###Permission Frequency
To ensure that users are not asked for permission too frequently, our API ensures that calling clevertap.notifications.push does nothing if the user has rejected the dialog less than one week ago. Once a week has passed by, calling the function will display the dialog as expected. This eliminates the need for you to implement a separate check before calling the function. 

To set a separate frequency, send "askAgainTimeInSeconds":time_in_seconds along with the above object. 

To skip the CleverTap pop-up, send "skipDialog":true along with the object above.

## Migrating from Other Services
This section is applicable for all browsers such as, Chrome, Firefox, and Safari. If you are migrating from a different web push notifications provider, you may be concerned about asking subscribed users to enable notifications a second time.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "If a user has already enabled notifications on your host domain, calling clevertap.notifications.push will not display the pop-up for that particular user and the registration will automatically be migrated to our new implementation."
}
[/block]
You can use the custom service file below which should only be used during the migration phase. Once the token migration of most of your users is complete, you can replace it with the [parent service worker](https://developer.clevertap.com/docs/web#section-add-the-service-worker-file). 
[block:code]
{
  "codes": [
    {
      "code": "importScripts('https://s3-eu-west-1.amazonaws.com/static.wizrocket.com/js/sw_webpush_only_registration.js');",
      "language": "javascript"
    }
  ]
}
[/block]
## Debugging

This section is applicable for all browsers such as, Chrome, Firefox, and Safari. Error messages and warnings are logged to the JS console of the browser.

For verbose logging, enable verbose logging of all communication with the CleverTap servers by setting the WZRK_D variable in sessionStorage. In the developer console of your browser type, use: sessionStorage['WZRK_D'] = '';
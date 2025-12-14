---
title: Troubleshooting Push Notifications
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
# GCM Token Invalid Error

GCM registers every device for a unique device token to enable remote push notification to be sent via CleverTap. Check the format of the GCM project number and registration token you pass on to CleverTap. It should be an exact match to the token a user receives from GCM. Do not truncate or add additional characters.

Inside the <application></application> tags, register the built-in broadcast receiver, and specify your GCM project number, replacing com.your.package with your application’s package name:
[block:code]
{
  "codes": [
    {
      "code": "<!-- replace below value with your GCM project number. Separate multiple sender IDs with a comma -->\n<meta-data android:name=\"GCM_SENDER_ID\" android:value=\"id:1234567890\"/> \n\n<meta-data\n    android:name=\"com.google.android.gms.version\"\n    android:value=\"@integer/google_play_services_version\"/>\n\n<receiver\n    android:name=\"com.clevertap.android.sdk.GcmBroadcastReceiver\"\n    android:permission=\"com.google.android.c2dm.permission.SEND\">\n    <intent-filter>\n        <action\n            android:name=\"com.google.android.c2dm.intent.RECEIVE\"/>\n        <action\n            android:name=\"com.google.android.c2dm.intent.REGISTRATION\"/>\n        <category android:name=\"com.your.package\"/>\n    </intent-filter>\n</receiver>\n",
      "language": "xml"
    }
  ]
}
[/block]
If you have your custom implementation for managing push notifications, you can inform CleverTap about the user’s GCM registration ID.


[block:code]
{
  "codes": [
    {
      "code": "String gcmRegId = gcm.register(projectNumber);\ncleverTap.data.pushGcmRegistrationId(gcmRegId, true);\n",
      "language": "java"
    }
  ]
}
[/block]
# Missing Device Token Error 

On registration, all devices in a particular app environment receive a unique device token to enable remote push notifications. Missing device token is encountered when the device in question isn’t associated with a registration token from GCM/APNS. This could be due to following reasons:
  * The device has not received a token from GCM/APNS or has not pushed the registration token to CleverTap due to infrequent network connectivity. Since the process is asynchronous, it may take longer in some cases to register a device as push enabled.
  * For GCM you can check the request you make to acquire the registration token (ensure the request contains a registration token i.e registration_id in plain text or registration_ids in JSON).
  * For APNS you can verify if you’re using the correct development environment against the .p12 certificate uploaded. Eg. If you’re using a sandbox Xcode build with a production token certificate or vice versa.

# GCM Sender ID Mismatch Error

Sender ID Mismatch is a common error which occurs when you use your Android application Project ID instead of the project number.

To identify and resolve this issue, you need to confirm the following things:
  * The GCMSender in the config.properties file of your Android Projects code is the project number and not the ID, it will look something like this – gcmSender = 123456789123 (it should not contain quotes or any other special characters)
  * The GCM API Key you have entered in the CleverTap Dashboard is the Key for server apps. This is available in the API Manager Section of the Developers Console, it is an alphanumeric string.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/249a650-error1.png",
        "error1.png",
        1122,
        663,
        "#faf8f7"
      ]
    }
  ]
}
[/block]
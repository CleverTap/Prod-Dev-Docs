---
title: Troubleshooting Push Notifications
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# FCM Token Invalid Error

FCM registers every device for a unique device token to enable remote push notifications to be sent via CleverTap. Check the format of the FCM Sender ID and registration token you pass on to CleverTap. It should be an exact match to the token a user receives from FCM. Do not truncate or add additional characters.

Inside the <application></application> tags, specify your FCM Sender ID:
[block:code]
{
  "codes": [
    {
      "code": "<!-- replace below value with your FCM Sender id -->\n<meta-data android:name=\"FCM_SENDER_ID\" android:value=\"id:1234567890\"/>",
      "language": "xml"
    }
  ]
}
[/block]
If you have your custom implementation for managing push notifications, you can inform CleverTap about the user’s FCM registration ID.


[block:code]
{
  "codes": [
    {
      "code": "cleverTap.pushFcmRegistrationId(fcmRegId, true);",
      "language": "java"
    },
    {
      "code": "cleverTap.pushFcmRegistrationId(fcmRegId, true)",
      "language": "text",
      "name": "Kotlin"
    }
  ]
}
[/block]
# Missing Device Token Error 

On registration, all devices in a particular app environment receive a unique device token to enable remote push notifications. The missing device token is encountered when the device in question isn’t associated with a registration token from FCM/APNS. This could be due to the following reasons:
  * The device has not received a token from FCM/APNS or has not pushed the registration token to CleverTap due to infrequent network connectivity. Since the process is asynchronous, it may take longer in some cases to register a device as push enabled.
  * For FCM you can check the request you make to acquire the registration token (ensure the request contains a registration token i.e registration_id in plain text or registration_ids in JSON).
  * For APNS you can verify if you’re using the correct development environment against the .p12 certificate uploaded. Eg. If you’re using a sandbox Xcode build with a production token certificate or vice versa.

# FCM Sender ID Mismatch Error

Sender ID Mismatch is a common error that occurs when you use your Android application Project ID instead of the project number.

To identify and resolve this issue, you need to confirm the following things:
  * The FCMSender in the config.properties file of your Android Projects code is the project number and not the ID, it will look something like this – fcmSender = 123456789123 (it should not contain quotes or any other special characters)
  * The FCM API Key you have entered in the CleverTap Dashboard is the Key for server apps. This is available in the API Manager section of the Developers Console, it is an alphanumeric string.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7c76d71-project_settings_fcm.png",
        "Add server key on the CleverTap dashboard.",
        2134,
        886,
        "#f3f4f7"
      ],
      "border": true,
      "caption": "Add Server Key"
    }
  ]
}
[/block]
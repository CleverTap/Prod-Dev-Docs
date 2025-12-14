---
title: How to Create an iOS APNs Auth Key
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
If you’d like to send push notifications to your iOS users, you will need to upload either an APNs Push Certificate, or an APNs Auth Key.

We recommend that you create and upload an APNs Auth Key for the following reasons:
  * No need to re-generate the push certificate every year
  * One auth key can be used for all your apps – this avoids the complication of maintaining different certificates

When sending push notifications using an APNs Auth Key, we require the following information about your app:
  * Auth Key file
  * Team ID
  * Your app’s bundle ID

To create an APNs auth key, follow the steps below.

Visit the Apple [Developer Member Center](https://developer.apple.com/account/).
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/019fb11-api1.png",
        "api1.png",
        2880,
        1800,
        "#f9f9f9"
      ]
    }
  ]
}
[/block]
Click on “Certificates, Identifiers & Profiles”.

Go to Keys from the Left Menu.

Create a new Auth Key by clicking on the “+” button in the top right corner.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a402056-api2.png",
        "api2.png",
        2880,
        1800,
        "#eeeeee"
      ]
    }
  ]
}
[/block]
On the following page, add a Key Name, and select APNs


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/08011a6-api3.png",
        "api3.png",
        2880,
        1800,
        "#f2f1f1"
      ]
    }
  ]
}
[/block]
Hit the Continue button.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d707f9d-api4.png",
        "api4.png",
        2880,
        1800,
        "#f2f3f3"
      ]
    }
  ]
}
[/block]
On this page, you will be able to download your auth key file. Please do not rename this file, and upload it as it is to our dashboard, as shown later below.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b3a6b0e-api5.png",
        "api5.png",
        2880,
        1800,
        "#f2f2f3"
      ]
    }
  ]
}
[/block]
Locate and copy your Team ID – click on your name/company name at the top right corner, then select “View Account”.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0d2a17a-api6.png",
        "api6.png",
        1200,
        650,
        "#fafafa"
      ]
    }
  ]
}
[/block]
Copy your Team ID, and head over to the CleverTap dashboard, and navigate to Settings -> Mobile Push -> iOS.

Upload the APNs Auth Key file while you downloaded earlier, and paste your Team ID and your app’s bundle ID. Your app’s bundle ID can be found in Xcode.

Also, select the appropriate mode for sending push notifications, i.e. either Production or Development.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5a3943b-12.png",
        "12.png",
        2752,
        1800,
        "#f7f8f9"
      ]
    }
  ]
}
[/block]
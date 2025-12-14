---
title: Add a Sound File to your Android App
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
This guide will show you how you can add a sound file to your Android app, thereby allowing you to use custom sounds for your Android push notifications.

Android supports .mp3, .ogg and .wav files for playing custom sounds.

# Step 1: Adding Sound Files to Your Android Project

Right click on Resources (res) folder and select New -> Android Resource Directory.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9573c35-and1.png",
        "and1.png",
        1550,
        484,
        "#e2e3e6"
      ]
    }
  ]
}
[/block]
Select Resource Type as “raw”.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1e3d59b-and2.png",
        "and2.png",
        1268,
        729,
        "#eaedf1"
      ]
    }
  ]
}
[/block]
Put your MP3 file in the raw folder.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fe92de6-and3.png",
        "and3.png",
        788,
        1074,
        "#e4e6e8"
      ]
    }
  ]
}
[/block]
# Step 2 : Registering Notification Channel For the Custom Sound File


[block:code]
{
  "codes": [
    {
      "code": "// Creating a Notification Channel With Sound Support\nCleverTapAPI.createNotificationChannel(getApplicationContext(),\"got\",\"Game of Thrones\",\"Game Of Thrones\",NotificationManager.IMPORTANCE_MAX,true,\"gameofthrones.mp3\");\n\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.createNotificationChannel(getApplicationContext(),\"got\",\"Game of Thrones\",\"Game Of Thrones\",NotificationManager.IMPORTANCE_MAX,true,\"gameofthrones.mp3\")",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

[block:api-header]
{}
[/block]
# Step 3: Creating Push Notifications with Custom Sounds

In the CleverTap dashboard, select Advanced → Custom Sound file and enter the name of your file with the extension in the text box. 

[block:callout]
{
  "type": "info",
  "body": "If the notification channel has a custom sound, then this sound will always override the default OS sound.",
  "title": "Note"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/56b4dd8-19.png",
        "19.png",
        1956,
        380,
        "#f6f7f9"
      ]
    }
  ]
}
[/block]
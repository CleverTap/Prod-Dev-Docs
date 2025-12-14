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
# Step 2: Creating Push Notifications with Custom Sounds

In the CleverTap dashboard, select Advanced → Custom Sound file and enter the name of your file with the extension in the text box.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/656bf24-and4.png",
        "and4.png",
        1600,
        997,
        "#f6f7f8"
      ]
    }
  ]
}
[/block]
---
title: mParticle
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
#Introduction

You can integrate mParticle with CleverTap to start receiving data from the app SDK. This will require integrating a CleverTap kit with the mParticle SDK.


#Android Integration

You can download the CleverTap kit from the [CleverTap Android repository](https://github.com/mparticle-integrations/mparticle-android-integration-clevertap#clevertap-kit-integration). 

A CleverTap kit is an integration that provides additional client-side add-on libraries to the [mParticle Android SDK](https://github.com/mParticle/mparticle-android-sdk).

1. Add the kit dependency to your app's build.gradle file:
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    compile 'com.mparticle:android-clevertap-kit:5+'\n}\n",
      "language": "javascript"
    }
  ]
}
[/block]
2. Follow the mParticle Android SDK [Quick-Start Guide](https://github.com/mParticle/mparticle-android-sdk), then rebuild and launch your app, and verify that you see `"CleverTap detected"` in the output of the `adb logcat`.
3. Enable [mParticle integration](https://docs.mparticle.com/integrations/clevertap/event/).


# iOS Integration

You can download the CleverTap kit from the [CleverTap iOS repository](https://github.com/mparticle-integrations/mparticle-apple-integration-clevertap).

This repository contains the [CleverTap](https://clevertap.com/) integration for the [mParticle Apple SDK](https://github.com/mParticle/mparticle-apple-sdk).

1. Add the kit dependency to your app's Podfile or Cartfile:
[block:code]
{
  "codes": [
    {
      "code": "    pod 'mParticle-CleverTap', '~> 7.0'\n    ",
      "language": "text"
    }
  ]
}
[/block]
    OR
[block:code]
{
  "codes": [
    {
      "code": "  github \"mparticle-integrations/mparticle-apple-integration-clevertap\" ~> 7.0\n  ",
      "language": "text"
    }
  ]
}
[/block]


2. Follow the mParticle iOS SDK [quick-start](https://github.com/mParticle/mparticle-apple-sdk), then rebuild and launch your app, and verify that you see `"CleverTap detected"` in the output of the `adb logcat`.

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The mParticle log level must be Debug or higher."
}
[/block]
3. Enable [mParticle integration](https://docs.mparticle.com/integrations/clevertap/event/).
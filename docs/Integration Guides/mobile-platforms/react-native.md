---
title: React Native
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: install-react-native
      title: Install React Native
    - type: basic
      slug: integrate-react-native
      title: Integrate React Native
    - type: link
      title: Example Usage for JS
      url: https://developer.clevertap.com/docs/react-native#section-example-usage
---
# Install and Integrate

1.  Install using `npm install --save clevertap-react-native`.
2. Follow the [installation instructions](https://developer.clevertap.com/docs/install-react-native).
3. Follow the [integration instructions](https://developer.clevertap.com/docs/integrate-react-native).

# Example Usage

## Grab a reference  
Create a CleverTap reference to use Javascript methods
[block:code]
{
  "codes": [
    {
      "code": "const CleverTap = require('clevertap-react-native');\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
## Record an event  
Method to push an event to CleverTap
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.recordEvent('testEvent');",
      "language": "javascript"
    }
  ]
}
[/block]
## Update a user profile  
Method to push user profile to CleverTap
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.onUserLogin({'Name': 'testUserA1', 'Identity': '123456', 'Email': 'test@test.com', 'custom1': 123});\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
##Implementation setLocation
Method to pass user location information to CleverTap
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.setLocation(34.15, -118.20);\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
## Create a Notification Channel
Method to create Notification Channels in Android
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.createNotificationChannel(\"yourChannelId\",\"YourChannelName\",\"YourChannelDescription\",3,true);",
      "language": "javascript"
    }
  ]
}
[/block]
## For more examples of JavaScript : 
  *  [see the included Example Project](https://github.com/CleverTap/clevertap-react-native/blob/master/Starter/App.js) 
  *  [see the CleverTap JS interface](https://github.com/CleverTap/clevertap-react-native/blob/master/index.js)
  *  [see the CleverTap TS interface](https://github.com/CleverTap/clevertap-react-native/blob/master/index.d.ts)

## Additional Resources
  * [CleverTap Android SDK Integration guide](https://support.clevertap.com/docs/android/getting-started.html)
  * [CleverTap iOS SDK Integration guide](https://support.clevertap.com/docs/ios/getting-started.html)
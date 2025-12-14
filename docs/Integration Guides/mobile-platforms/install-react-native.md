---
title: Install React Native
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
      slug: integrate-react-native
      title: Integrate React Native
---
CleverTap provides a [React Native SDK](https://github.com/CleverTap/clevertap-react-native) that enables app developers to track, segment, and engage their users. 

# Install React Native SDK

Run the following command:
[block:code]
{
  "codes": [
    {
      "code": "npm install --save clevertap-react-native\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
#Link Library

Linking of libraries is not required for projects using React Native version 0.60 and higher. See [Autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) for more information.

# Link the Project (React Native version 0.59 and lower)
[block:callout]
{
  "type": "warning",
  "body": "Linking a project is required only for React Native version 0.59 and lower.",
  "title": "Note"
}
[/block]
##Link Automatically 
Run the following command to link the project automatically: 
[block:code]
{
  "codes": [
    {
      "code": "react-native link clevertap-react-native\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "This command is valid for iOS and Android."
}
[/block]
##Link Manually

 You can manually link the project by following these steps for each platform:

### Link iOS Project
1. Drag and Drop node_modules/clevertap-react-native/ios/CleverTapReact.xcodeproj into the Libraries folder of your project in XCode ([see Step 1 here](https://reactnative.dev/docs/linking-libraries-ios.html#step-1-1)). 

2. Drag and Drop the libCleverTapReact.a product in CleverTapReact.xcodeproj into your project's target's "Link Binary With Libraries" section ([see Step 2 here](https://reactnative.dev/docs/linking-libraries-ios.html#step-2-1)).

3. Add a Header Search Path pointing to `$(SRCROOT)/../node_modules/clevertap-react-native/ios` ([see Step 3 here](https://reactnative.dev/docs/linking-libraries-ios.html#step-3)).
  
###Link Android Project
Add the following code in your android/settings.gradle file:
[block:code]
{
  "codes": [
    {
      "code": "include ':clevertap-react-native'\nproject(':clevertap-react-native').projectDir = new File(settingsDir, '../node_modules/clevertap-react-native/android')\n  \n  \n  ",
      "language": "java"
    }
  ]
}
[/block]
Add the following code in your android/app/build.gradle file:
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    ...\n    compile project(':clevertap-react-native')\n}\n\n\n",
      "language": "java"
    }
  ]
}
[/block]
# Platform-Specific Install Steps

##**Install for iOS with podspec**
If your iOS project is using CocoaPods,  add `pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'` as a dependency in your ios/Podfile.

For your iOS, add the following to your Podfile:

[block:code]
{
  "codes": [
    {
      "code": "target 'YOUR_TARGET_NAME' do  \n    use_frameworks!\n    pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'\nend\n      \n      ",
      "language": "objectivec"
    }
  ]
}
[/block]
Run `pod install` from your iOS directory.

##**Install for iOS without podspec**
If your iOS project does not use CocoaPods,  add pod 'CleverTap-iOS-SDK' as a dependency in your ios/Podfile. See an example Podfile [here](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/Podfile).
[block:code]
{
  "codes": [
    {
      "code": "cd ios; pod install --repo-update.",
      "language": "curl",
      "name": "Bash"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "After pod install, open your project using **[MyProject].xcworkspace ** instead of the original .xcodeproj.",
  "title": "Note"
}
[/block]
##Troubleshooting for iOS
If you are using React Native 0.60 or if your project configuration does not allow adding `use_frameworks!` in the podfile, then you can add `use_modular_headers!` to enable the stricter search paths and module map generation for all of your pods. Alternatively, you can add `:modular_headers => true` to a single pod declaration to enable only that pod.

##**Install for Android**
Add the clevertap-android-sdk and firebase-messaging (if you wish to support push notifications) packages in your android/app/build.gradlefile.
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n\t...\n    implementation 'com.clevertap.android:clevertap-android-sdk:3.7.2'\n    implementation 'com.google.android.gms:play-services-base:16.0.1'\n    implementation 'com.google.firebase:firebase-messaging:17.3.3'\n    implementation 'com.google.android.exoplayer:exoplayer:2.8.4' //Optional for Audio/Video\n    implementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4' //Optional for Audio/Video\n    implementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4' //Optional for Audio/Video\n    implementation 'com.github.bumptech.glide:glide:4.9.0' //Mandatory for App Inbox\n    implementation 'com.android.support:design:28.0.0' //Mandatory for App Inbox\n    implementation \"com.android.support:appcompat-v7:28.0.0\" //Mandatory for App Inbox\n  implementation 'com.android.installreferrer:installreferrer:1.0'//Mandatory for CleverTap Android SDK v3.6.4 and above add the following\n\n    //Note - ExoPlayer dependencies are optional but all 3 are required for Audio/Video Inbox and InApp Messages\n}\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
##Troubleshooting for Android
If you see the `java.lang.UnsatisfiedLinkError: couldn't find DSO to load: libhermes.so` error at runtime, 1. Add the following code in your app/build.gradle:
[block:code]
{
  "codes": [
    {
      "code": "project.ext.react = [\n    entryFile: \"index.js\",\n    enableHermes: false //add this\n]\ndef jscFlavor = 'org.webkit:android-jsc:+'\ndef enableHermes = project.ext.react.get(\"enableHermes\", false);\ndef jscFlavor = 'org.webkit:android-jsc:+'\ndef enableHermes = project.ext.react.get(\"enableHermes\", false);\ndependencies {\nif (enableHermes) {\n      // For RN 0.60.x\n      def hermesPath = \"../../node_modules/hermesvm/android/\"\n      debugImplementation files(hermesPath + \"hermes-debug.aar\")\n      releaseImplementation files(hermesPath + \"hermes-release.aar\")\n    } else {\n      implementation jscFlavor\n    }\n}",
      "language": "javascript"
    }
  ]
}
[/block]
2. Add the following code In android/build.gradle:
[block:code]
{
  "codes": [
    {
      "code": "maven {\n        url \"$rootDir/../node_modules/jsc-android/dist\"\n      }\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
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

```javascript
npm install --save clevertap-react-native
```

# Link Library

Linking of libraries is not required for projects using React Native version 0.60 and higher. See [Autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) for more information.

# Link the Project (React Native version 0.59 and lower)

> 🚧 Note
>
> Linking a project is required only for React Native version 0.59 and lower.

## Link Automatically 

Run the following command to link the project automatically: 

```javascript
react-native link clevertap-react-native
```

> 📘 Note
>
> This command is valid for iOS and Android.

## Link Manually

 You can manually link the project by following these steps for each platform:

### Link iOS Project

1. Drag and Drop node\_modules/clevertap-react-native/ios/CleverTapReact.xcodeproj into the Libraries folder of your project in XCode ([see Step 1 here](https://reactnative.dev/docs/linking-libraries-ios.html#step-1-1)). 

2. Drag and Drop the libCleverTapReact.a product in CleverTapReact.xcodeproj into your project's target's "Link Binary With Libraries" section ([see Step 2 here](https://reactnative.dev/docs/linking-libraries-ios.html#step-2-1)).

3. Add a Header Search Path pointing to `$(SRCROOT)/../node_modules/clevertap-react-native/ios` ([see Step 3 here](https://reactnative.dev/docs/linking-libraries-ios.html#step-3)).

### Link Android Project

Add the following code in your android/settings.gradle file:

```java
include ':clevertap-react-native'
project(':clevertap-react-native').projectDir = new File(settingsDir, '../node_modules/clevertap-react-native/android')
```

Add the following code in your android/app/build.gradle file:

```java
dependencies {
    ...
    compile project(':clevertap-react-native')
}
```

# Platform-Specific Install Steps

## **Install for iOS with podspec**

If your iOS project is using CocoaPods,  add `pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'` as a dependency in your ios/Podfile.

For your iOS, add the following to your Podfile:

```objectivec
target 'YOUR_TARGET_NAME' do  
    use_frameworks!
    pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'
end
```

Run `pod install` from your iOS directory.

## **Install for iOS without podspec**

If your iOS project does not use CocoaPods,  add pod 'CleverTap-iOS-SDK' as a dependency in your ios/Podfile. See an example Podfile [here](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/Podfile).

```curl Bash
cd ios; pod install --repo-update.
```

> 📘 Note
>
> After pod install, open your project using **[MyProject].xcworkspace** instead of the original .xcodeproj.

## Troubleshooting for iOS

If you are using React Native 0.60 or if your project configuration does not allow adding `use_frameworks!` in the podfile, then you can add `use_modular_headers!` to enable the stricter search paths and module map generation for all of your pods. Alternatively, you can add `:modular_headers => true` to a single pod declaration to enable only that pod.

## **Install for Android**

Add the clevertap-android-sdk and firebase-messaging (if you wish to support push notifications) packages in your android/app/build.gradlefile.

```javascript
dependencies {
	...
    implementation 'com.clevertap.android:clevertap-android-sdk:3.7.2'
    implementation 'com.google.android.gms:play-services-base:16.0.1'
    implementation 'com.google.firebase:firebase-messaging:17.3.3'
    implementation 'com.google.android.exoplayer:exoplayer:2.8.4' //Optional for Audio/Video
    implementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4' //Optional for Audio/Video
    implementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4' //Optional for Audio/Video
    implementation 'com.github.bumptech.glide:glide:4.9.0' //Mandatory for App Inbox
    implementation 'com.android.support:design:28.0.0' //Mandatory for App Inbox
    implementation "com.android.support:appcompat-v7:28.0.0" //Mandatory for App Inbox
  implementation 'com.android.installreferrer:installreferrer:1.0'//Mandatory for CleverTap Android SDK v3.6.4 and above add the following

    //Note - ExoPlayer dependencies are optional but all 3 are required for Audio/Video Inbox and InApp Messages
}
```

## Troubleshooting for Android

If you see the `java.lang.UnsatisfiedLinkError: couldn't find DSO to load: libhermes.so` error at runtime, 1. Add the following code in your app/build.gradle:

```javascript
project.ext.react = [
    entryFile: "index.js",
    enableHermes: false //add this
]
def jscFlavor = 'org.webkit:android-jsc:+'
def enableHermes = project.ext.react.get("enableHermes", false);
def jscFlavor = 'org.webkit:android-jsc:+'
def enableHermes = project.ext.react.get("enableHermes", false);
dependencies {
if (enableHermes) {
      // For RN 0.60.x
      def hermesPath = "../../node_modules/hermesvm/android/"
      debugImplementation files(hermesPath + "hermes-debug.aar")
      releaseImplementation files(hermesPath + "hermes-release.aar")
    } else {
      implementation jscFlavor
    }
}
```

2. Add the following code In android/build.gradle:

```javascript
maven {
        url "$rootDir/../node_modules/jsc-android/dist"
      }
```

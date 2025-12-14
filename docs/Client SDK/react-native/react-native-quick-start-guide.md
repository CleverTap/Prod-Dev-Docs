---
title: React Native Quick Start Guide
excerpt: Learn how to integrate your React Native application.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: React Native Push
      url: https://developer.clevertap.com/docs/react-native-push-notification
    - type: link
      title: React Native In-App
      url: https://developer.clevertap.com/docs/react-native-in-app
    - type: link
      title: React Native App Inbox
      url: https://developer.clevertap.com/docs/react-native-app-inbox
    - type: link
      title: React Native Advanced Features
      url: https://developer.clevertap.com/docs/react-native-advanced-features
    - type: link
      title: React Native Resources
      url: https://developer.clevertap.com/docs/react-native-resources
---
# Overview

This document outlines the installation, setup, and testing of your React Native application.  

> 📘 React Native New Architecture Support
> 
> Starting from [clevertap-react-native v3.0.0](https://www.npmjs.com/package/clevertap-react-native), CleverTap supports the [React Native New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page), while still maintaining compatibility with the old architecture. 
> 
> To know more about the New Architecture and how to enable it in your app, refer to the [React Native Official Guide](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/enable-apps.md).
> 
> Before migrating, ensure that all your dependencies are compatible with the New Architecture to avoid any integration issues.

# Install

To add the CleverTap React Native SDK to your project, run this command from the terminal:

```shell
npm install --save clevertap-react-native
```

# Android Integration

## Add CleverTap Credentials

To associate your Android app with your CleverTap account, you must add your CleverTap credentials to the application's `AndroidManifest.xml` file. To add CleverTap credentials, add your CleverTap Account ID and Token to your `AndroidManifest.xml` within the `<application></application>` tags.

```xml
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="Your CleverTap Account ID"/>
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="Your CleverTap Account Token"/>
<!-- IMPORTANT: To force use Google AD ID to uniquely identify  users, use the following meta tag. GDPR mandates that if you are using this tag, there is prominent disclousure to your end customer in their application. Read more about GDPR here - https://clevertap.com/blog/in-preparation-of-gdpr-compliance/ -->
<meta-data
    android:name="CLEVERTAP_USE_GOOGLE_AD_ID"
    android:value="1"/>
```

> 🚧 Credentials and Access
> 
> The **CleverTap Account ID** and **CleverTap Token** are available as **Project ID** and **Project Token** respectively on the CleverTap dashboard. Member and Creator roles in the project do not have access to view the Passcode and Project Token of the account on the dashboard.

## Add Region Code

To know how to add region code for Android in React Native SDK, refer to [Region Codes.](doc:idc#android)

## Enable Tracking by Adding Permissions

Add the following snippet in the `AndroidManifest.xml` file so the CleverTap SDK can access the internet:

```xml
<!-- Required to allow the app to send events and user profile information -->
<uses-permission android:name="android.permission.INTERNET"/>
<!-- Recommended so that CleverTap knows when to attempt a network call -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Add Dependencies

1. Add the following code to your `dependencies` section in `project/build.gradle`:

```groovy
dependencies {
            classpath 'com.android.tools.build:gradle:7.2.1'
            classpath 'com.google.gms:google-services:4.3.3' //<--- Mandatory for using Firebase Messaging, skip if not using FCM
        }
```

2. Add the following to your `dependencies` section in `app/build.gradle`:

> 📘 Migrating from ExoPlayer to AndroidX Media3
> 
> Starting from CleverTap React Native SDK v3.0.0, CleverTap now supports [AndroidX Media3](https://developer.android.com/media/media3/exoplayer/migration-guide), replacing the deprecated ExoPlayer libraries. CleverTap continues to support [ExoPlayer](https://github.com/google/ExoPlayer), but for a smoother migration to AndroidX Media3, update the following dependencies:

```groovy Dependencies with AndroidX Media3
implementation 'com.clevertap.android:clevertap-android-sdk:7.0.2'
implementation 'com.google.firebase:firebase-messaging:23.0.6'
implementation 'androidx.core:core:1.4.0'
implementation 'androidx.fragment:fragment:1.3.6'
        
//MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'
        
//For CleverTap Android SDK v3.6.4 and above add the following -
implementation 'com.android.installreferrer:installreferrer:2.2'

//Optional AndroidX Media3 Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "androidx.media3:media3-exoplayer:1.1.1"
implementation "androidx.media3:media3-exoplayer-hls:1.1.1"
implementation "androidx.media3:media3-ui:1.1.1"
```
```groovy Dependencies with Exoplayer Libraries
implementation 'com.clevertap.android:clevertap-android-sdk:7.0.2'
implementation 'com.google.firebase:firebase-messaging:23.0.6'
implementation 'androidx.core:core:1.4.0'
implementation 'androidx.fragment:fragment:1.3.6'
        
//MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'
        
//For CleverTap Android SDK v3.6.4 and above add the following -
implementation 'com.android.installreferrer:installreferrer:2.2'
        
//Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "com.google.android.exoplayer:exoplayer:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-hls:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-ui:2.19.1"
```

3. Apply the following plugin at the end of the `app/build.gradle` file:

```groovy
apply plugin: 'com.google.gms.google-services' //skip if not using FCM
```

## Set Up Android Application Class

1. Add the CleverTapPackage to the react-native package list from the app's `Android Application class`:

```kotlin
// CleverTap imports
import com.clevertap.react.CleverTapPackage

// ...

// Add CleverTapPackage to the React Native package list
override fun getPackages(): List<ReactPackage> {
    val packages = PackageList(this).packages
    // Packages that cannot be autolinked yet can be added manually here, for example:
    packages.add(CleverTapPackage()) // only needed when not auto-linking
    return packages
}
```
```java
// CleverTap imports
import com.clevertap.react.CleverTapPackage;

// ...

// Add CleverTapPackage to the React Native package list
@Override
protected List<ReactPackage> getPackages() {
    List<ReactPackage> packages = new PackageList(this).getPackages();
    // Packages that cannot be autolinked yet can be added manually here, for example:
    packages.add(new CleverTapPackage()); // Only needed when auto-linking is not used
    return packages;
}
```

2. Extend your `Android Application class` from `CleverTapApplication` to enable out-of-the-box integration.

```kotlin
import com.clevertap.react.CleverTapApplication
import com.clevertap.android.sdk.ActivityLifecycleCallback
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.CleverTapAPI.LogLevel
import com.facebook.react.ReactApplication
// other imports

class MainApplication : CleverTapApplication(),
    ActivityLifecycleCallbacks,
    ReactApplication {

    override fun onCreate() {
        super.onCreate()  // Call base implementation first

        CleverTapAPI.setDebugLevel(LogLevel.VERBOSE)
        // ActivityLifecycleCallback.register(this) // Required only for v2.2.1 and below

        // ...
    }
}
```
```java
import com.clevertap.react.CleverTapApplication;
import com.clevertap.android.sdk.ActivityLifecycleCallback;
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.CleverTapAPI.LogLevel;
import com.facebook.react.ReactApplication;
// other imports

public class MainApplication extends CleverTapApplication
        implements ActivityLifecycleCallbacks, ReactApplication {

    @Override
    public void onCreate() {
        super.onCreate(); // best practice to call super first

        CleverTapAPI.setDebugLevel(LogLevel.VERBOSE);
        // ActivityLifecycleCallback.register(this); // Required only for v2.2.1 and below

        // ...
    }
}
```

## (Optional) Set Up Android Main Activity Class

To notify CleverTap of a launch deep link, override the `onCreate()` in the `MainActivity.java` file:

```kotlin
import android.os.Bundle
import com.clevertap.react.CleverTapRnAPI

class MainActivity : ReactActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        CleverTapRnAPI.setInitialUri(intent?.data) // From v3.0.0+
    }

    // ...
}
```
```java
import com.clevertap.react.CleverTapRnAPI;
import android.os.Bundle;


public class MainActivity extends ReactActivity {
    // ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        CleverTapRnAPI.setInitialUri(getIntent().getData()); // From v3.0.0+
	}

    // ...
}
```

# iOS Integration

## Integration with podspec

To integrate the CleverTap React Native SDK with your iOS project using a `podspec`:

1. Add the following dependency in your `iOS/Podfile`:

```ruby
pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'
```

2. Ensure your Podfile is configured as follows:

```ruby
target 'YOUR_TARGET_NAME' do  
    use_frameworks!
    pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'
end
```

3. Navigate to your iOS directory and run the following command:

```Text Bash
pod install
```

4. (Optional) To enable Turbo Module Architecture in your project, reinstall your pods by running the `RCT_NEW_ARCH_ENABLED=1 bundle exec pod install` command. For more information, refer to [Turbo Module Architecture Installation](doc:react-native-quick-start-guide#turbo-module-architecture-installation).

## Integration without podspec

1. To integrate CleverTap without using a `podspec`, add the following dependency to your `iOS/Podfile`:

```ruby
pod 'CleverTap-iOS-SDK'
```

2. Run the following command from your iOS directory:

```Text Bash
cd ios && pod install --repo-update
```

3. (Optional) To enable Turbo Module Architecture in your project, reinstall your pods by running the `RCT_NEW_ARCH_ENABLED=1 bundle exec pod install` command. For more information, refer to [Turbo Module Architecture Installation](doc:react-native-quick-start-guide#turbo-module-architecture-installation).

### Turbo Module Architecture Installation

Starting from v3.0.0, CleverTap now supports the new Turbo Module Architecture. For more information, refer to the [React Native New Architecture](https://reactnative.dev/docs/the-new-architecture/landing-page).

> 📘 Opening Your Project After Pod Installation
> 
> After running `pod install`, make sure to open your project using **[MyProject].xcworkspace** instead of the original `.xcodeproj` file.

## Add CleverTap Credentials

To associate your iOS app with your CleverTap account, you need to add your CleverTap credentials in the `Info.plist` file in your application. To do so:

1. Go to the `Info` file from your project navigator.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/369c9426ff6da75e285b1417f9b873bd7fe7bc5522a8c5a68897b2cfc6f8ef03-image.png",
        null,
        "Information Property List in Info"
      ],
      "align": "center",
      "sizing": "75% ",
      "border": true,
      "caption": "Information Property List in Info"
    }
  ]
}
[/block]


2. Create a key called `CleverTapAccountID` of type _String_. 
3. Create a key called `CleverTapToken` of type _String_.
4. Insert the _Project ID_ and account _Project Token_ values from your [CleverTap account](https://eu1.dashboard.clevertap.com). These values are available under the _Settings_ page.

> 🚧 Credentials and Access
> 
> The `CleverTapAccountId` and `CleverTapToken` are available as _Project ID_ and _Project Token_ respectively on the CleverTap dashboard.  
> Member and Creator roles in the project don’t have access to view the _Passcode_ and _Project Token_ of the account on the dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d94b29494952222e470323189c78cbf1b06c801762290adef237ca419581e759-image.png",
        null,
        "CleverTap Project Credentials"
      ],
      "align": "center",
      "sizing": "75% ",
      "border": true,
      "caption": "CleverTap Project Credentials"
    }
  ]
}
[/block]


## Add Region Code

To know how to add region code for iOS in React Native SDK, refer to [Region Codes.](doc:idc#ios)

# Initialize CleverTap SDK

Add the CleverTap SDK to your iOS project to enable user engagement tracking and campaign personalization. To do so, follow these steps:

1. Intialize CleverTap SDK by adding the following code snippet to your `AppDelegate` file

```swift
import CleverTapSDK
import CleverTapReact
```
```objectivec Objective-C
#import <CleverTap-iOS-SDK/CleverTap.h>
#import <Clevertap-react-native/CleverTapReactManager.h>
```

2. Notify the CleverTap React Native SDK of the application launch in your `didFinishLaunchingWithOptions:` method:

```swift
CleverTap.autoIntegrate() // integrate CleverTap SDK using the autoIntegrate option
CleverTapReactManager.sharedInstance()?.applicationDidLaunch(options: launchOptions)
```
```objectivec
[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option
[[CleverTapReactManager sharedInstance] applicationDidLaunchWithOptions:launchOptions];
```

> 📘 Note
> 
> Need to use `@import CleverTapSDK;` instead of `#import <CleverTap-iOS-SDK/CleverTap.h>` and `@import CleverTapReact;` instead of `#import <clevertap-react-native/CleverTapReactManager.h>` in the AppDelegate class in case if using `use_modular_headers!` in the podfile.

# Example Usage

## Grab a reference

Create a CleverTap reference to use JavaScript methods:

```javascript
const CleverTap = require('clevertap-react-native');
```

## Track User Profiles

Create a User profile when the user logs in (On User Login):

```javascript JavaScript
CleverTap.onUserLogin({'Name': 'React-Test', 'Identity': '11102008', 'Email': 'r@gmail.com', 'custom1': 43});
```

## Update the User Profile

The `Onuserlogin` method identifies individual users on a device when the User logs in for the first time. However, you may need to add additional user properties to the user profile such as gender, date of birth, and so on. You can update these properties with the `profilePush()` method. For more information, refer to our [User Documentation](https://developer.clevertap.com/docs/concepts-user-profiles).

## Track User Events

Record an event without event data:

```javascript JavaScript
CleverTap.recordEvent('testEvent');
```

Record an event with event data:

```javascript
CleverTap.recordEvent(
    'Product Viewed', {
        'Product Name': 'Dairy Milk',
        'Category': 'Chocolate',
        'Amount': 20.00
    });
```

# TroubleShooting

## Android

If you face the following crash at runtime:

```diff Logcat
java.lang.UnsatisfiedLinkError: couldn't find DSO to load: libhermes.so
```

Add the following to your `app/build.gradle`:

```groovy
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

In `android/build.gradle` add this:

```groovy
maven {
        url "$rootDir/../node_modules/jsc-android/dist"
      }
```

## iOS

If you are on React-Native v0.60 or your project configuration doesn't allow you to add `use_frameworks!` in the podfile, you can add `use_modular_headers!` to enable the stricter search paths and module map generation for all of your pods. Alternatively, you can add`:modular_headers => true` to a single pod declaration to enable for only that pod.

## GDPR Compliance

CleverTap provides APIs to support GDPR compliance. For more information, refer to [Advance Features](https://developer.clevertap.com/docs/react-native-advanced-features#app-personalization).
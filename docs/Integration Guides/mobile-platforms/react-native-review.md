---
title: React Native - Review - Parth
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
CleverTap provides a [React Native SDK](https://github.com/CleverTap/clevertap-react-native) that enables app developers to track, segment, and engage their users. 

# Install React SDK

Run  `npm install --save clevertap-react-native`.

> 🚧 Note
>
> Linking is not required for Projects using React Native version 0.60 and higher. See [Autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) for more information.

# Disable auto-linking in Android for React Native version 0.60 and higher

To disable auto-linking in Android, add a react-native.config.js file to the project root (where the package.json is available) to prevent CleverTap package from auto-linking:

```java
module.exports = {
dependencies: {
 	'clevertap-react-native': {
  	 platforms: {
 	  	 ios: null,
  	  	 android: null,
 	       },
     },
   },
};
```

# Link the Project using React Native version 0.59 and lower

> 🚧 Note
>
> Linking a project is required only for React Native version 0.59 and lower.

Run the following command to link the project automatically: `react-native link clevertap-react-native`.

# Manually link the Project

 You can manually, link the project by following these steps for each platform.

## **iOS**

* Drag and Drop node\_modules/clevertap-react-native/ios/CleverTapReact.xcodeproj into the Libraries folder of your project in XCode [see Step 1 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
* Drag and Drop the libCleverTapReact.a product in CleverTapReact.xcodeproj into your project's target's "Link Binary With Libraries" section [see Step 2 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
* Add a Header Search Path pointing to `$(SRCROOT)/../node_modules/clevertap-react-native/ios` [see Step 3 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).

## **Android**

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

## **iOS with podspec**

Add `pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'` as a dependency in your ios/Podfile.

For your iOS, add the following to your Podfile:

```objectivec
target 'YOUR_TARGET_NAME' do  
    use_frameworks!
    pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'
end
```

Run `pod install` from your iOS directory.

## **iOS without podspec**

Add pod 'CleverTap-iOS-SDK' as a dependency in your ios/Podfile. See an example Podfile [here](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/Podfile).

```curl Bash
cd ios; pod install --repo-update.
```

Note that after pod install, open your project using **[MyProject].xcworkspace** instead of the original .xcodeproj.

## Troubleshooting for iOS

If you are using React Native 0.60 or if your project configuration does not allow adding `use_frameworks!` in the podfile, then you can add `use_modular_headers!` to enable the stricter search paths and module map generation for all of your pods. Alternatively, you can add `:modular_headers => true` to a single pod declaration to enable only that pod.

## **Android**

Add the clevertap-android-sdk and firebase-messaging (if you wish to support push notifications) packages in your android/app/build.gradlefile.

```java
dependencies {
	...
    compile 'com.clevertap.android:clevertap-android-sdk:3.6.0'
    compile 'com.google.android.gms:play-services-base:16.0.1'
    compile 'com.google.firebase:firebase-messaging:17.3.3'
    compile 'com.google.android.exoplayer:exoplayer:2.8.4' //Optional for Audio/Video
    compile 'com.google.android.exoplayer:exoplayer-hls:2.8.4' //Optional for Audio/Video
    compile 'com.google.android.exoplayer:exoplayer-ui:2.8.4' //Optional for Audio/Video
    compile 'com.github.bumptech.glide:glide:4.9.0' //Mandatory for App Inbox
    compile 'com.android.support:design:28.0.0' //Mandatory for App Inbox
    compile "com.android.support:appcompat-v7:28.0.0" //Mandatory for App Inbox

    //Note - ExoPlayer dependencies are optional but all 3 are required for Audio/Video Inbox and InApp Messages
}
```

### Troubleshooting steps for Android

If you see the `java.lang.UnsatisfiedLinkError: couldn't find DSO to load: libhermes.so` error at runtime, 1. Add the following code in your app/build.gradle:

```java
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

2. In android/build.gradle add the following code:

```java
maven {
        url "$rootDir/../node_modules/jsc-android/dist"
      }
```

# Platform-Specific Integration Steps

Integrate the CleverTap SDK into your iOS and Android apps.

## **iOS**

* Follow the integration instructions starting with Step 2 [here](doc:ios-quickstart-guide).
* In your AppDelegate didFinishLaunchingWithOptions: notify the CleverTap React SDK of application launch:

```objectivec
[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option
[[CleverTapReactManager sharedInstance] applicationDidLaunchWithOptions:launchOptions];
```

> 🚧 Note
>
> Add the CleverTap imports at the top of the file.

```objectivec
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapReact/CleverTapReactManager.h>
```

Here is an [example project](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/ExampleProject/AppDelegate.m).

## **Android**

* Follow the integration instructions starting with Step 2 [here](doc:android-quickstart-guide).

The CleverTap SDK is not yet upgraded to AndroidX. Add the following to your gradle.properties file:

```java
android.useAndroidX=true
 android.enableJetifier=true
```

* Add CleverTapPackage to the packages list in MainApplication.java (android/app/src/\[...]/MainApplication.java)

```java
// ...

// CleverTap imports
import com.clevertap.android.sdk.ActivityLifecycleCallback;
import com.clevertap.react.CleverTapPackage;

//...

// add CleverTapPackage to react-native package list
@Override
  protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
            new MainReactPackage(),
            new CleverTapPackage(), // <-- add this

// ...

// add onCreate() override
@Override
public void onCreate() {
   // Register the CleverTap ActivityLifecycleCallback; before calling super
  ActivityLifecycleCallback.register(this);	
  super.onCreate();
}
```

* Optionally, override onCreate in MainActivity.java to notify CleverTap of a launch deep link (android/app/src/\[...]/MainActivity.java).

```java
import com.clevertap.react.CleverTapModule;

public class MainActivity extends ReactActivity {
	// ...

	@Override
	protected void onCreate(Bundle savedInstanceState) {
    	super.onCreate(savedInstanceState);
    	CleverTapModule.setInitialUri(getIntent().getData());
	}

    // ...
}
```

Here is an [example project](https://github.com/CleverTap/clevertap-react-native/tree/master/ExampleProject/android/app/src/main).

### Example JS Usage

### Grab a reference

`const CleverTap = require('clevertap-react-native');`

#### Record an event

`CleverTap.recordEvent('testEvent');`

#### Update a user profile

`CleverTap.profileSet({'Name': 'testUserA1', 'Identity': '123456', 'Email': 'test@test.com', 'custom1': 123});`

### Examples

To see how to use the CleverTap React SDK in your app, please visit the links below:

* [Example Project](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ExampleProject.js) 
* [CleverTap JS interface](https://github.com/CleverTap/clevertap-react-native/blob/master/index.js)

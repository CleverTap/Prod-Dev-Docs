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
---
CleverTap provides a [React Native SDK](https://github.com/CleverTap/clevertap-react-native) that enables app developers to track, segment, and engage their users. 

# Install React SDK

Run  `npm install --save clevertap-react-native`.

# Link the Project

Run the following command to link the project automatically: `react-native link clevertap-react-native`.

Alternatively, you can manually, link the project by following these steps for each platform.

**iOS**

* Drag and Drop node\_modules/clevertap-react-native/ios/CleverTapReact.xcodeproj into the Libraries folder of your project in XCode [see Step 1 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
* Drag and Drop the libCleverTapReact.a product in CleverTapReact.xcodeproj into your project's target's "Link Binary With Libraries" section [see Step 2 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
* Add a Header Search Path pointing to `$(SRCROOT)/../node_modules/clevertap-react-native/ios` [see Step 3 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).

**Android**

Add the code below in your android/settings.gradle file.

```java
include ':clevertap-react-native'
project(':clevertap-react-native').projectDir = new File(settingsDir, '../node_modules/clevertap-react-native/android')
```

Add the code below in your android/app/build.gradle file.

```java
dependencies {
    ...
    compile project(':clevertap-react-native')
}
```

# Platform-Specific Install Steps

**iOS**\
Add pod 'CleverTap-iOS-SDK' as a dependency in your ios/Podfile. See an example Podfile [here](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/Podfile).

```curl Bash
cd ios; pod install --repo-update.
```

Note that after pod install, open your project using [MyProject].xcworkspace instead of the original .xcodeproj.

**Android**\
Add the clevertap-android-sdk and firebase-messaging (if you wish to support push notifications) packages in your android/app/build.gradlefile.

```java
dependencies {
	...
    compile 'com.clevertap.android:clevertap-android-sdk:3.1.8'
    compile 'com.google.android.gms:play-services-base:11.4.0'
    compile 'com.google.firebase:firebase-messaging:11.4.0'
}
```

# Platform-Specific Integration Steps

Now, you will need to integrate the CleverTap SDK into your iOS and Android apps.

**iOS**

* Follow the integration instructions starting with Step 2 [here](doc:ios-quickstart-guide).
* In your AppDelegate didFinishLaunchingWithOptions: notify the CleverTap React SDK of application launch:

```objectivec
[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option
[[CleverTapReactManager sharedInstance] applicationDidLaunchWithOptions:launchOptions];
```

NOTE: Don't forget to add the CleverTap imports at the top of the file.

```objectivec
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapReact/CleverTapReactManager.h>
```

Here is an [example project](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/ExampleProject/AppDelegate.m).

**Android**

* Follow the integration instructions starting with Step 2 [here](doc:android-quickstart-guide).
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

# Example JS Usage

### Grab a reference

`const CleverTap = require('clevertap-react-native');`

### Record an event

`CleverTap.recordEvent('testEvent');`

### Update a user profile

`CleverTap.profileSet({'Name': 'testUserA1', 'Identity': '123456', 'Email': 'test@test.com', 'custom1': 123});`

# Examples

To see how to use the CleverTap React SDK in your app, please visit the links below:

* [Example Project](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ExampleProject.js) 
* [CleverTap JS interface](https://github.com/CleverTap/clevertap-react-native/blob/master/index.js)

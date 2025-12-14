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
  * Drag and Drop node_modules/clevertap-react-native/ios/CleverTapReact.xcodeproj into the Libraries folder of your project in XCode [see Step 1 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
  * Drag and Drop the libCleverTapReact.a product in CleverTapReact.xcodeproj into your project's target's "Link Binary With Libraries" section [see Step 2 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
  * Add a Header Search Path pointing to `$(SRCROOT)/../node_modules/clevertap-react-native/ios` [see Step 3 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).

**Android**

Add the code below in your android/settings.gradle file.
[block:code]
{
  "codes": [
    {
      "code": "include ':clevertap-react-native'\nproject(':clevertap-react-native').projectDir = new File(settingsDir, '../node_modules/clevertap-react-native/android')",
      "language": "java"
    }
  ]
}
[/block]
Add the code below in your android/app/build.gradle file.
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    ...\n    compile project(':clevertap-react-native')\n}",
      "language": "java"
    }
  ]
}
[/block]
# Platform-Specific Install Steps

**iOS**
Add pod 'CleverTap-iOS-SDK' as a dependency in your ios/Podfile. See an example Podfile [here](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/Podfile).
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
Note that after pod install, open your project using [MyProject].xcworkspace instead of the original .xcodeproj.

**Android**
Add the clevertap-android-sdk and firebase-messaging (if you wish to support push notifications) packages in your android/app/build.gradlefile.
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n\t...\n    compile 'com.clevertap.android:clevertap-android-sdk:3.1.8'\n    compile 'com.google.android.gms:play-services-base:11.4.0'\n    compile 'com.google.firebase:firebase-messaging:11.4.0'\n}",
      "language": "java"
    }
  ]
}
[/block]
# Platform-Specific Integration Steps

Now, you will need to integrate the CleverTap SDK into your iOS and Android apps.

**iOS**
* Follow the integration instructions starting with Step 2 [here](doc:ios-quickstart-guide).
* In your AppDelegate didFinishLaunchingWithOptions: notify the CleverTap React SDK of application launch:
[block:code]
{
  "codes": [
    {
      "code": "[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option\n[[CleverTapReactManager sharedInstance] applicationDidLaunchWithOptions:launchOptions];",
      "language": "objectivec"
    }
  ]
}
[/block]
NOTE: Don't forget to add the CleverTap imports at the top of the file.
[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTap.h>\n#import <CleverTapReact/CleverTapReactManager.h>",
      "language": "objectivec"
    }
  ]
}
[/block]
Here is an [example project](https://github.com/CleverTap/clevertap-react-native/blob/master/ExampleProject/ios/ExampleProject/AppDelegate.m).

**Android**
* Follow the integration instructions starting with Step 2 [here](doc:android-quickstart-guide).
* Add CleverTapPackage to the packages list in MainApplication.java (android/app/src/[...]/MainApplication.java)
[block:code]
{
  "codes": [
    {
      "code": "// ...\n\n// CleverTap imports\nimport com.clevertap.android.sdk.ActivityLifecycleCallback;\nimport com.clevertap.react.CleverTapPackage;\n\n//...\n\n// add CleverTapPackage to react-native package list\n@Override\n  protected List<ReactPackage> getPackages() {\n    return Arrays.<ReactPackage>asList(\n            new MainReactPackage(),\n            new CleverTapPackage(), // <-- add this\n\n// ...\n\n// add onCreate() override\n@Override\npublic void onCreate() {\n   // Register the CleverTap ActivityLifecycleCallback; before calling super\n  ActivityLifecycleCallback.register(this);\t\n  super.onCreate();\n}",
      "language": "java"
    }
  ]
}
[/block]
* Optionally, override onCreate in MainActivity.java to notify CleverTap of a launch deep link (android/app/src/[...]/MainActivity.java).
[block:code]
{
  "codes": [
    {
      "code": "import com.clevertap.react.CleverTapModule;\n\npublic class MainActivity extends ReactActivity {\n\t// ...\n\n\t@Override\n\tprotected void onCreate(Bundle savedInstanceState) {\n    \tsuper.onCreate(savedInstanceState);\n    \tCleverTapModule.setInitialUri(getIntent().getData());\n\t}\n\n    // ...\n}",
      "language": "java"
    }
  ]
}
[/block]
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
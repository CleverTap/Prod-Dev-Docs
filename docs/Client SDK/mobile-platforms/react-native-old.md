---
title: React Native_OLD
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


[block:callout]
{
  "type": "warning",
  "body": "Linking is not required for Projects using React Native version 0.60 and higher. See [Autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) for more information.",
  "title": "Note"
}
[/block]
#Disable auto-linking in Android for React Native version 0.60 and higher

To disable auto-linking in Android, add a react-native.config.js file to the project root (where the package.json is available) to prevent CleverTap package from auto-linking:

[block:code]
{
  "codes": [
    {
      "code": "module.exports = {\ndependencies: {\n \t'clevertap-react-native': {\n  \t platforms: {\n \t  \t ios: null,\n  \t  \t android: null,\n \t       },\n     },\n   },\n};",
      "language": "java"
    }
  ]
}
[/block]
# Link the Project using React Native version 0.59 and lower
[block:callout]
{
  "type": "warning",
  "body": "Linking a project is required only for React Native version 0.59 and lower.",
  "title": "Note"
}
[/block]
Run the following command to link the project automatically: `react-native link clevertap-react-native`.

#Manually link the Project
 You can manually, link the project by following these steps for each platform.

##**iOS**
  * Drag and Drop node_modules/clevertap-react-native/ios/CleverTapReact.xcodeproj into the Libraries folder of your project in XCode [see Step 1 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
  * Drag and Drop the libCleverTapReact.a product in CleverTapReact.xcodeproj into your project's target's "Link Binary With Libraries" section [see Step 2 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).
  * Add a Header Search Path pointing to `$(SRCROOT)/../node_modules/clevertap-react-native/ios` [see Step 3 here](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking).

##**Android**

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

##**iOS with podspec**
Add `pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'` as a dependency in your ios/Podfile.

For your iOS, add the following to your Podfile:

[block:code]
{
  "codes": [
    {
      "code": "target 'YOUR_TARGET_NAME' do  \n    use_frameworks!\n    pod 'clevertap-react-native', :path => '../node_modules/clevertap-react-native'\nend\n      \n      ",
      "language": "ruby"
    }
  ]
}
[/block]
Run `pod install` from your iOS directory.

##**iOS without podspec**
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
Note that after pod install, open your project using **[MyProject].xcworkspace ** instead of the original .xcodeproj.

##Troubleshooting for iOS
If you are using React Native 0.60 or if your project configuration does not allow adding `use_frameworks!` in the podfile, then you can add `use_modular_headers!` to enable the stricter search paths and module map generation for all of your pods. Alternatively, you can add `:modular_headers => true` to a single pod declaration to enable only that pod.

##**Android**
Add the clevertap-android-sdk and firebase-messaging (if you wish to support push notifications) packages in your android/app/build.gradlefile.
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n\t...\n    implementation 'com.clevertap.android:clevertap-android-sdk:3.6.4'\n    implementation 'com.google.android.gms:play-services-base:16.0.1'\n    implementation 'com.google.firebase:firebase-messaging:17.3.3'\n    implementation 'com.google.android.exoplayer:exoplayer:2.8.4' //Optional for Audio/Video\n    implementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4' //Optional for Audio/Video\n    implementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4' //Optional for Audio/Video\n    implementation 'com.github.bumptech.glide:glide:4.9.0' //Mandatory for App Inbox\n    implementation 'com.android.support:design:28.0.0' //Mandatory for App Inbox\n    implementation \"com.android.support:appcompat-v7:28.0.0\" //Mandatory for App Inbox\n    //Mandatory for v0.3.9 and above\n\t\timplementation \"com.android.installreferrer:installreferrer:1.0\"\n    //Note - ExoPlayer dependencies are optional but all 3 are required for Audio/Video Inbox and InApp Messages\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]
###Troubleshooting steps for Android
If you see the `java.lang.UnsatisfiedLinkError: couldn't find DSO to load: libhermes.so` error at runtime, 1. Add the following code in your app/build.gradle:
[block:code]
{
  "codes": [
    {
      "code": "project.ext.react = [\n    entryFile: \"index.js\",\n    enableHermes: false //add this\n]\ndef jscFlavor = 'org.webkit:android-jsc:+'\ndef enableHermes = project.ext.react.get(\"enableHermes\", false);\ndef jscFlavor = 'org.webkit:android-jsc:+'\ndef enableHermes = project.ext.react.get(\"enableHermes\", false);\ndependencies {\nif (enableHermes) {\n      // For RN 0.60.x\n      def hermesPath = \"../../node_modules/hermesvm/android/\"\n      debugImplementation files(hermesPath + \"hermes-debug.aar\")\n      releaseImplementation files(hermesPath + \"hermes-release.aar\")\n    } else {\n      implementation jscFlavor\n    }\n}",
      "language": "java"
    }
  ]
}
[/block]
2. In android/build.gradle add the following code:
[block:code]
{
  "codes": [
    {
      "code": "maven {\n        url \"$rootDir/../node_modules/jsc-android/dist\"\n      }\n\n",
      "language": "java"
    }
  ]
}
[/block]
# Platform-Specific Integration Steps

Integrate the CleverTap SDK into your iOS and Android apps.

##**iOS**
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

[block:callout]
{
  "type": "warning",
  "body": "Add the CleverTap imports at the top of the file.",
  "title": "Note"
}
[/block]

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

##**Android**
* Follow the integration instructions starting with Step 2 [here](doc:android-quickstart-guide).

The CleverTap SDK is not yet upgraded to AndroidX. Add the following to your gradle.properties file:
[block:code]
{
  "codes": [
    {
      "code": "android.useAndroidX=true\n android.enableJetifier=true\n  \n  ",
      "language": "java"
    }
  ]
}
[/block]
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
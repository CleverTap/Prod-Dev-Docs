---
title: Integrate React Native
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: After integrating the project, refer to the examples on the main page.
  pages:
    - type: link
      title: Example Usage
      url: https://developer.clevertap.com/docs/react-native#section-example-usage
---
# Platform-Specific Integration Steps

Integrate the CleverTap SDK into your iOS and Android apps.

##**iOS**
* Follow the integration instructions starting with step 2 in the [iOS Quick Start Guide](https://developer.clevertap.com/docs/ios-quickstart-guide#section-step-2-add-clever-tap-credentials).
* In your AppDelegate `didFinishLaunchingWithOptions:` notify the CleverTap React Native SDK of application launch:
[block:code]
{
  "codes": [
    {
      "code": "[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option\n[[CleverTapReactManager sharedInstance] applicationDidLaunchWithOptions:launchOptions];\n\n",
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
      "code": "#import <CleverTapSDK/CleverTap.h>\n#import <CleverTapReact/CleverTapReactManager.h>\n  \n  ",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "If you are using `use_modular_headers!` in the podfile then you must import following in your AppDelegate class:\n1.@import CleverTapSDK;\n2.@import CleverTapReact;"
}
[/block]
##**Android**
* Follow the integration instructions starting with step 2 in the [Android Quick Start Guide](https://developer.clevertap.com/docs/android-quickstart-guide#section-step-2-add-your-clever-tap-credentials-in-android-manifest-xml).

The CleverTap SDK is not yet upgraded to AndroidX. Add the following to your `gradle.properties` file:
[block:code]
{
  "codes": [
    {
      "code": "android.useAndroidX=true\nandroid.enableJetifier=true\n  \n  ",
      "language": "xml"
    }
  ]
}
[/block]
* Add CleverTapPackage to the packages list in MainApplication.java (android/app/src/[...]/MainApplication.java)
[block:code]
{
  "codes": [
    {
      "code": "// ...\n\n// CleverTap imports\nimport com.clevertap.android.sdk.ActivityLifecycleCallback;\nimport com.clevertap.react.CleverTapPackage;\n\n//...\n\n// add CleverTapPackage to react-native package list\n@Override\n  protected List<ReactPackage> getPackages() {\n    return Arrays.<ReactPackage>asList(\n            new MainReactPackage(),\n            new CleverTapPackage(), // <-- add this\n\n// ...\n\n// add onCreate() override\n@Override\npublic void onCreate() {\n   // Register the CleverTap ActivityLifecycleCallback; before calling super\n  ActivityLifecycleCallback.register(this);\t\n  super.onCreate();\n}\n    \n    ",
      "language": "javascript"
    }
  ]
}
[/block]
* Optionally, override onCreate in MainActivity.java to notify CleverTap of a launch deep link (android/app/src/[...]/MainActivity.java).
[block:code]
{
  "codes": [
    {
      "code": "import com.clevertap.react.CleverTapModule;\n\npublic class MainActivity extends ReactActivity {\n\t// ...\n\n\t@Override\n\tprotected void onCreate(Bundle savedInstanceState) {\n    \tsuper.onCreate(savedInstanceState);\n    \tCleverTapModule.setInitialUri(getIntent().getData());\n\t}\n\n    // ...\n}\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
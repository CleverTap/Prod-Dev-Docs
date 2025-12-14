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

## **iOS**

* Follow the integration instructions starting with step 2 in the [iOS Quick Start Guide](https://developer.clevertap.com/docs/ios-quickstart-guide#section-add-clever-tap-credentials).
* In your AppDelegate `didFinishLaunchingWithOptions:` notify the CleverTap React Native SDK of application launch:

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

> 📘 Note
>
> If you are using `use_modular_headers!` in the podfile then you must import following in your AppDelegate class:\
> 1.\@import CleverTapSDK;\
> 2.\@import CleverTapReact;

## **Android**

* Follow the integration instructions starting with step 2 in the [Android Quick Start Guide](https://developer.clevertap.com/docs/android-quickstart-guide#section-step-2-add-your-clever-tap-credentials-in-android-manifest-xml).

The CleverTap SDK is not yet upgraded to AndroidX. Add the following to your `gradle.properties` file:

```xml
android.useAndroidX=true
android.enableJetifier=true
```

* Add CleverTapPackage to the packages list in MainApplication.java (android/app/src/\[...]/MainApplication.java)

```javascript
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

```javascript
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

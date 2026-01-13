---
title: Android Advanced Features
excerpt: Learn more about advanced Android features.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: android-resources
      title: Android Resources
---
# Overview

Add advanced features to your app, such as event tracking, location reporting, data opt-outs, push notification support, and support for Play Store privacy compliance.

The SDK also supports [PII Encryption](doc:android-advanced-features#encryption-of-pii-data) at rest and in transit, ensuring that sensitive user data such as Email, Identity, Name, and Phone is securely stored and transmitted over the network, offering end-to-end data protection.

# Opt Out

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, we provide the following method using which the app can stop sending events to CleverTap.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOptOut(true) // Will opt out the user to send data to CleverTap
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOptOut(true); // Will opt out the user to send data to CleverTap
```

From CleverTap Android SDK 7.5.0 onwards, the `setOptOut()` method supports an enhanced signature that allows apps to stop tracking events while sending generic, non-personalized communications, in compliance with GDPR guidelines.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOptOut(true, true) // Only stop tracking, allow generic communication
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOptOut(true, true); // Only stop tracking, allow generic communication
```

If the user wants to opt in again to share their data, you call the same method like this.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOptOut(false) // Will opt in the user to send data to CleverTap
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOptOut(false); // Will opt in the user to send data to CleverTap
```

By default, the SDK will send data to CleverTap, and the opt-out will be set to false. It would be a good practice to capture the opt-out flag of the user and set it whenever the app launches.

# Enable Device Networking Info Reporting

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, the CleverTap Android SDK will not capture any kind of personal information like WiFi, Bluetooth, Network Information and user IP information. To enable the capturing of all this information, you can call the following method:

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.enableDeviceNetworkInfoReporting(true)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.enableDeviceNetworkInfoReporting(true);
```

To disable Device Network information to be sent to CleverTap you can use the same method as follows.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.enableDeviceNetworkInfoReporting(false)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.enableDeviceNetworkInfoReporting(false);
```

By default, CleverTap does not collect device network information.

# Set Offline

CleverTap Android SDK provides a method to set the user as offline. Setting the user offline means that no events/profile information will be sent to CleverTap. You can set the user as offline using the following method

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOffline(true) // Set user offline (pause data transmission)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOffline(true);
```

You can set the user back online using the following method

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOffline(false) // Set user online (enable real-time data transmission)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOffline(false);
```

# Encryption of PII Data

PII data is stored across the SDK and could contain sensitive information. From CleverTap SDK v5.2.0 onwards, you can enable encryption for PII data such as Email, Identity, Name, Phone,  and so on.

CleverTap supports three encryption levels for PII data:

* **NONE (0)**: Data is stored in plaintext.
* **MEDIUM (1)**: Only PII fields are encrypted.
* **FULL_DATA (2)**: Encrypts all stored data, not just PII fields. Available from Android SDK version 7.7.0 and above.

> 📘 Default Encryption Level
>
> By default, the encryption level is set to **NONE (0)**.

### Set Encryption Level for the Default Instance

To configure encryption for the default CleverTap instance, add the following metadata entry in your `AndroidManifest.xml` file:

```xml
<meta-data
    android:name="CLEVERTAP_ENCRYPTION_LEVEL"
           android:value="2" />
  <!-- Use value="1" for Medium encryption level -->
    <!-- If not specified, default is 0 (None) -->
```

### Set Encryption Level for Additional Instances

Different instances can have different encryption levels. To set an encryption level for an additional instance:

```kotlin
// Create configuration for an additional CleverTap instance
val clevertap = CleverTapInstanceConfig.createInstance(
    applicationContext,
    "ADDITIONAL_CLEVERTAP_ACCOUNT_ID",
    "ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN"
).apply {
    // Set encryption level - choose only one

    // For Medium encryption
    setEncryptionLevel(EncryptionLevel.MEDIUM)

    // For Full Data Encryption (v7.7.0+)
    setEncryptionLevel(EncryptionLevel.FULL_DATA)
}.let { config ->
    CleverTapAPI.instanceWithConfig(applicationContext, config)
}
```
```java
CleverTapInstanceConfig clevertapAdditionalInstanceConfig = CleverTapInstanceConfig.createInstance(
    applicationContext,
    "ADDITIONAL_CLEVERTAP_ACCOUNT_ID",
    "ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN"
)
// Set encryption level (choose one based on your requirements)

// For Medium encryption
clevertapAdditionalInstanceConfig.setEncryptionLevel(EncryptionLevel.MEDIUM);

// For Full Data Encryption (available in SDK v7.7.0+)
clevertapAdditionalInstanceConfig.setEncryptionLevel(EncryptionLevel.FULL_DATA);

// Initialize CleverTap API with the additional config
CleverTapAPI clevertap = CleverTapAPI.instanceWithConfig(applicationContext, clevertapAdditionalInstanceConfig);
```

## Encryption in Transit

Encrypting data in transit ensures that sensitive personal data, such as Email, Identity, Name, Phone, and so on, is encrypted before being sent over the network. This protects user data during transmission and adds an extra layer of security beyond the at-rest encryption.

From CleverTap Android SDK 7.5.0 onwards, this feature is supported natively in the SDK, with encryption and decryption handled automatically.

> 📘 Private Beta
>
> This feature is currently released in Private Beta. To enable it, contact CleverTap Support. Check that it is activated for your account before implementing it in the SDK.

### Encryption Scope

Encryption in transit applies to all event data.

### Prerequisites for Encryption In Transit

To use this feature, ensure that you perform the following steps:

1. Contact your account manager to enable the encryption feature on your account.
2. Set the string key `CLEVERTAP_ENCRYPTION_IN_TRANSIT` to `1` in your app’s `AndroidManifest.xml`.

### Encryption Specifications

The following table outlines the encryption specifications used by the CleverTap SDK:

| Parameter      | Value                                |
| -------------- | ------------------------------------ |
| Algorithm      | AES-256 (symmetric)                  |
| IV Size        | 12 bytes (96 bits)                   |
| Encoding       | Base64 for all encrypted content     |
| Key Management | Base64 direct (v0), RSA planned (v1) |

### Error Scenarios

If encryption in transit fails, the SDK returns one of the following error codes:

| Error Code | Error Message                                                   | Description                                                                                                                                                    |
| ---------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `402`      | `"Encryption In Transit disabled. Retry with plain text"`       | The encryption feature is not enabled. To proceed, either enable Encryption In Transit in your CleverTap account or contact your Account Manager to enable it. |
| `419`      | `"Encryption In Transit request failed, retry with plain text"` | Encryption header missing.                                                                                                                                     |
| `400`      | `"Invalid JSON payload"`                                        | Malformed request.                                                                                                                                             |

# Referral Tracking

From CleverTap SDK 3.6.4 onwards, add the following Gradle dependency to capture UTM details, app-install time, referrer click time, and other metrics provided by Google Install Referrer Library.

```groovy
implementation 'com.android.installreferrer:installreferrer:2.2'
```

# Debugging

Follow the steps for debugging your app:

1. By default, CleverTap logs are set to CleverTap.Loglevel.INFO. We recommend that you set the SDK to `VERBOSE` mode to log warnings or other important messages to the Android logging system during development. This can be done by setting the debug level to `CleverTap.Loglevel.VERBOSE`. If you want to disable CleverTap logs for the production environment, you can set the debug level to `CleverTap.Loglevel.OFF`.

```kotlin Kotlin
CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO) //Default Log level

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG) //Set Log level to DEBUG log warnings or other important messages

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.VERBOSE) //Set Log level to VERBOSE

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF) //Switch off logs for Production environment}
```
```java JAVA
CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO);    //Default Log level

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);   //Set Log level to DEBUG log warnings or other important messages

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.VERBOSE);     //Set Log level to VERBOSE

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF); //Switch off logs for Production environment
```

2. After setting the debug level to `CleverTap.Loglevel.VERBOSE`, search for Clevertap.  The logcat displays the handshakes between CleverTap and your app.
3. Copy the CleverTap ID from the logs.
4. Open the CleverTap dashboard.
5. Navigate to _Segments_> _Find People_ > search _By Identity_.

<Image align="center" alt={1205} border={true} caption="Find People by Identity" title="Find People by Identity" src="https://files.readme.io/d46c9c8-Android_search_by__identity.png" />

## Changing Account Credentials

If you’d not want to insert your account credentials in your app’s AndroidManifest.xml, or would like to change your account ID programmatically, you need to create a custom Application class (if you don’t have one already) with the following content.

```kotlin Kotlin
import android.app.Application
import com.clevertap.android.sdk.ActivityLifecycleCallback
import com.clevertap.android.sdk.CleverTapAPI

class MyApplication:Application() {
  override fun onCreate() {
    CleverTapAPI.changeCredentials("Your account ID here", "Your account token here")
    ActivityLifecycleCallback.register(this) // Must be called before super.onCreate()
    super.onCreate()
  }
}
```
```java JAVA
import android.app.Application;
import com.clevertap.android.sdk.ActivityLifecycleCallback;
import com.clevertap.android.sdk.CleverTapAPI;

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        CleverTapAPI.changeCredentials("Your account ID here", "Your account token here");
        ActivityLifecycleCallback.register(this); // Must be called before super.onCreate()
        super.onCreate();
    }
}
```

If you’ve just created this subclass of Application, update your AndroidManifest.xml to specify your subclass.

```xml
<application
    android:label="@string/app_name"
    android:icon="@drawable/ic_launcher"
    android:name=".MyApplication">
```

If you have used this method, to set your CleverTap Account ID and Token, please do not add these values to your manifest file.

## Manually Updating User Location

The application is responsible for requesting the user’s permission to use location. If this permission is granted, location can be passed to CleverTap.

The App can pass the location to the SDK using setLocation.

```kotlin Kotlin
val cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext())
cleverTapAPI.setLocation(location) //android.location.Location}
```
```java JAVA
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
cleverTapAPI.setLocation(location); //android.location.Location
```

## Using Your Existing Activity Lifecycle Listener

If you’d like to use your own activity lifecycle listener, please update it to incorporate the following code.

```kotlin Kotlin
registerActivityLifecycleCallbacks(object : android.app.Application.ActivityLifecycleCallbacks {
            override fun onActivityCreated(activity: Activity, savedInstanceState: Bundle?) {
                CleverTapAPI.setAppForeground(true)
                try
                {
                    CleverTapAPI.getDefaultInstance(applicationContext)?.pushNotificationClickedEvent(activity.intent.extras)
                }
                catch (t:Throwable) {
                    // Ignore
                }
                try
                {
                    val intent = activity.intent
                    val data = intent.data
                    CleverTapAPI.getDefaultInstance(applicationContext)?.pushDeepLink(data)
                }
                catch (t:Throwable) {
                    // Ignore
                }
            }
            override fun onActivityStarted(activity: Activity) {
            }
            override fun onActivityResumed(activity: Activity) {
                try
                {
                    CleverTapAPI.onActivityResumed(activity)
                }
                catch (t:Throwable) {
                    // Ignore
                }
            }
            override fun onActivityPaused(activity: Activity) {
                try
                {
                    CleverTapAPI.onActivityPaused()
                }
                catch (t:Throwable) {
                    // Ignore
                }
            }
            override fun onActivityStopped(activity: Activity) {
            }

            override fun onActivitySaveInstanceState(activity: Activity, outState: Bundle){
            }
            override fun onActivityDestroyed(activity: Activity) {
            }
        })
```
```java JAVA
application.registerActivityLifecycleCallbacks(
        new android.app.Application.ActivityLifecycleCallbacks() {
        
            @Override
            public void onActivityCreated(Activity activity, Bundle bundle) {
                CleverTapAPI.setAppForeground(true);
                try {
                    CleverTapAPI.getDefaultInstance(application).pushNotificationClickedEvent(activity.getIntent().getExtras());
                } catch (Throwable t) {
                    // Ignore
                }
                try {
                    Intent intent = activity.getIntent();
                    Uri data = intent.getData();
                    CleverTapAPI.getDefaultInstance(application).pushDeepLink(data);
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityStarted(Activity activity) {
            }
            
            @Override
            public void onActivityResumed(Activity activity) {
                try {
                    CleverTapAPI.getDefaultInstance(application).onActivityResumed(activity);
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityPaused(Activity activity) {
                try {
                    CleverTapAPI.getDefaultInstance(application).onActivityPaused();
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityStopped(Activity activity) {
            }
            
            @Override
            public void onActivitySaveInstanceState(Activity activity, Bundle bundle) {
            }
            
            @Override
            public void onActivityDestroyed(Activity activity) {
            }
        }
);
```

# Update on Google Play Store privacy requirements

Refer to [Google play store privacy requirements](https://clevertap.com/blog/update-on-googleplay-store-privacy-requirements/).

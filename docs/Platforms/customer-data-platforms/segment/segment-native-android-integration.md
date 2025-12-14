---
title: Segment Native Android Integration
excerpt: Learn how to integrate Segment with CleverTap in Android.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Segment Native Android integration allows you to push your Android application data to CleverTap through Segment.

With this integration, you can:

- Push User Profiles
- Push Events
- Integrate Push Notifications

> ⚠️ End-of-Support Announcement: Analytics-Android SDK
> 
> The **Analytics-Android SDK** will reach end of support in March 2026. After this date, no new updates, security patches, or bug fixes will be provided.  
> 
> Segment is focusing future development on the **Analytics-Kotlin SDK**, which offers enhanced functionality and broader platform support.  
> 
> **CleverTap fully supports the Analytics-Kotlin integration.**  
> To migrate, refer to the following resources:
> 
> - [Segment Analytics-Kotlin Integration](doc:segment-analytics-kotlin-integration)  
> - [Analytics-Kotlin-CleverTap GitHub repository](https://github.com/CleverTap/analytics-kotlin-clevertap/tree/main)  
> - [Segment’s official migration guide](https://segment.com/docs/connections/sources/catalog/libraries/mobile/kotlin-android/migration/)

# Prerequisites

The following are the prerequisites for performing this integration:

- A CleverTap account
- A Segment account
- A functional Android application

# Integration

CleverTap Segment Android integration consists of the following major steps:

1. [Add Destination on Segment Dashboard](doc:segment-native-android-integration#add-destination-in-segment)
2. [Install SDK in the Native Application](doc:segment-native-android-integration#install-sdk-in-the-native-application)
3. [Initialize the Segment Client](doc:segment-native-android-integration#initialize-the-segment-client)

## Add Destination in Segment

Destinations are the business tools or applications to which Segment forwards your data. Adding Destinations allows you to act on your data and learn more about your customers in real-time.

To add CleverTap details in Segment:

1. Go to _Destinations_ in Segment application.
2. Click on _Add Destination_ and select _CleverTap_.
3. In the _Destination Catalog/CleverTap_ page, click on _Configure CleverTap_. 
4. Add CleverTap _Account ID_ and _Account Token_, which you can find on the CleverTap Dashboard under _Settings_ page.
5. Select the appropriate region from the _Region_ dropdown. To identify the region of your CleverTap account, refer to the following table:

| CleverTap Dashboard URL                                                                              | Region            |
| :--------------------------------------------------------------------------------------------------- | :---------------- |
| <https://in1.dashboard.clevertap.com/login.html#/>                                                   | IN                |
| <https://sg1.dashboard.clevertap.com/login.html#/>                                                   | SG                |
| <https://us1.dashboard.clevertap.com/login.html#/>                                                   | US                |
| <https://eu1.dashboard.clevertap.com/login.html#/>                                                   | None              |
| [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/) | Indonesia         |
| [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/) | Middle East (UAE) |

6. Turn on the toggle at the top of the _Settings_ page to enable the destination.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/de3fc69-2023-03-15_17-34-07.png",
        "Enable the CleverTap Settings",
        "Enable the CleverTap Settings"
      ],
      "align": "center",
      "caption": "Enable the CleverTap Settings"
    }
  ]
}
[/block]


You can integrate CleverTap from the server-side or mobile destination (iOS or Android). Select mobile destinations to use CleverTap push notifications or In-App notifications.

## Install SDK in the Native Application

To install Android SDK within the application, add the following dependencies in the `app/build.gradle` file: 

```groovy
dependencies {

    //segment and clevertap dependencies
    implementation 'com.segment.analytics.android:analytics:4.11.3'
    implementation 'com.clevertap.android:clevertap-android-sdk:7.4.1'
    implementation 'com.clevertap.android:clevertap-segment-android:1.7.0'
    
    //core android dependencies
    implementation 'androidx.core:core:1.13.1'
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'androidx.viewpager:viewpager:1.0.0'
    implementation 'androidx.fragment:fragment:1.3.6'
    implementation 'androidx.coordinatorlayout:coordinatorlayout:1.2.0'


    //google and 3rd party dependencies
    implementation 'com.google.firebase:firebase-messaging:24.0.3'
    implementation 'com.google.android.material:material:1.12.0'
    implementation 'com.google.android.exoplayer:exoplayer:2.19.1'//Mandatory if using App Inbox
    implementation 'com.google.android.exoplayer:exoplayer-hls:2.19.1'//Mandatory if using App Inbox
    implementation 'com.google.android.exoplayer:exoplayer-ui:2.19.1'//Mandatory if using App Inbox

    implementation 'com.github.bumptech.glide:glide:4.12.0'//Mandatory if using App Inbox
}
// at the end of the build.gradle file
apply plugin: 'com.google.gms.google-services'
```

## Initialize the Segment Client

To initialize the Segment client:

1. Add the following code to your Application class file:

```kotlin
class CleverTapSegmentApplication: Application() {
    private val TAG = String.format("%s.%s", "CLEVERTAP", CleverTapSegmentApplication::class.java.name)
    private val WRITE_KEY = "<Your_WRITE_KEY>" //This you will receive under source in segment.
    private val CLEVERTAP_KEY = "CleverTap"
    var sCleverTapSegmentEnabled = false
    private var clevertap: CleverTapAPI? = null

    override fun onCreate() {
        super.onCreate()

        CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG)

        val analytics = Analytics.Builder(applicationContext, WRITE_KEY)
            .logLevel(Analytics.LogLevel.VERBOSE)
            .use(CleverTapIntegration.FACTORY)
            .build()

        analytics.onIntegrationReady<CleverTapAPI>(CLEVERTAP_KEY) { instance ->
            Log.i(TAG, "analytics.onIntegrationReady() called")
            cleverTapIntegrationReady(instance)
        }
        Analytics.setSingletonInstance(analytics)
    }

    private fun cleverTapIntegrationReady(instance: CleverTapAPI) {
        instance.enablePersonalization()
        sCleverTapSegmentEnabled = true
        clevertap = instance
    }
}
```
```java
public class CleverTapSegmentApplication extends Application {

private static final String TAG = String.format("%s.%s", "CLEVERTAP", CleverTapSegmentApplication.class.getName());
private static final String WRITE_KEY = "<Your_WRITE_KEY>"; //This you will receive under source in segment. 
private static final String CLEVERTAP_KEY = "CleverTap";
public static boolean sCleverTapSegmentEnabled = false;
private CleverTapAPI clevertap;
private static Handler handler = null;

@Override 
public void onCreate() 
{
  super.onCreate();

  CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);

  Analytics analytics = new Analytics.Builder(getApplicationContext(), WRITE_KEY)
    .logLevel(Analytics.LogLevel.VERBOSE) 
    .use(CleverTapIntegration.FACTORY) 
    .build();

  analytics.onIntegrationReady(CLEVERTAP_KEY, new Analytics.Callback<CleverTapAPI>() 
  { 
    @Override
    public void onReady(CleverTapAPI instance) {
        Log.i(TAG, "analytics.onIntegrationReady() called");
        cleverTapIntegrationReady(instance); 
      }
    });
  Analytics.setSingletonInstance(analytics); 
}

  private void cleverTapIntegrationReady(CleverTapAPI instance) 
  { 
    instance.enablePersonalization();
    sCleverTapSegmentEnabled = true;
    clevertap = instance;
  } 
}
```

| Parameter        | Description                                                                                                                                                      |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `YOUR_WRITE_KEY` | From the Segment dashboard, navigate to _Connections > Sources_. Select the source and then navigate to _Settings_ > _API Keys_ to find the _Write Key_ details. |

2. Add the following permissions in the `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
```

# Push User Information

The `onUserLogin` method in CleverTap captures multiple user logins or signup on the same device. This method is useful when the user performs a login or sign-up, or wants to push data to CleverTap. Call the `identify` method in Segment to use the CleverTap `onUserLogin` method. 

```kotlin
val traits = Traits()
traits.putEmail("foo@foo.com")
traits.putName("FooName")
traits.putPhone("+14155551234")
Analytics.with(applicationContext).identify(61026032, traits, null)
//Replace all the example values with your required dynamic ones.
```
```java
Traits traits = new Traits();
traits.putEmail("foo@foo.com");
traits.putName("FooName");
traits.putPhone("+14155551234"); 
Analytics.with(getApplicationContext()).identify(61026032, traits, null);
//Replace all the example values with your required dynamic ones.
```

Pushing the user information applies only to Segment-CleverTap SDK version v1.1.2 and above.

> 📘 Update User Property to CleverTap
> 
> To update any user property to CleverTap, pass the same user identity passed during the user login or sign-up.

For more details, refer to [User Profile Considerations](doc:concepts-user-profiles#user-profile-consideration).

# Push Events

Events track users' actions in your application. To push events to the CleverTap dashboard, add the following code:

```kotlin
Analytics.with(applicationContext).track(
     "testEvent",
      Properties().putValue("value", "testValue").putValue("testDate", Date(System.currentTimeMillis()))
)
```
```java
Analytics.with(getApplicationContext()).track("testEvent",
new Properties().putValue("value", "testValue") .putValue("testDate", new Date(System.currentTimeMillis()))
);
```

Here, _testEvent_ is the Event Name and Value. _testDate _ is the Event Date Property.  
For more details,  refer to [Event Considerations](doc:events#event-consideration).

## Push Charged Events

When an event is tracked using the server-side destination with the name `Order Completed`(as per [e-commerce tracking API](https://segment.com/docs/connections/spec/ecommerce/v2/)), Segment maps that event to CleverTap’s charged event.  
To push charged events to the CleverTap dashboard, add the following code:

```kotlin
fun raiseChargedEvent(analytics: Analytics) {
        val properties = Properties()
        properties.putValue("orderId", "123456")
        properties.putValue("revenue", 100)
        properties.putProducts(
            Product("id1", "sku1", 100.0),
            Product("id2", "sku2", 200.0)
        )
        analytics.track("Order Completed", properties)
    }
```
```java
void raiseChargedEvent(Analytics analytics){
        Properties properties = new  Properties();
        properties.putValue("orderId", "123456");
        properties.putValue("revenue", 100);
        properties.putProducts(
                        new Properties.Product("id1", "sku1", 100.0),
                        new Properties.Product("id2", "sku2", 200.0)
                );
        analytics.track("Order Completed", properties);
    }
```

It appears as a charged event on the CleverTap dashboard, as shown in the following figure:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/72d0670-2023-01-12_15-57-27.png",
        "Push Charged Events",
        2742
      ],
      "align": "center",
      "border": true,
      "caption": "Charged Event on CleverTap Dashboard"
    }
  ]
}
[/block]


# Integrate Push Notifications

**Case 1**: When you are using push notifications only from CleverTap.  
  Add the following code to your `AndroidManifest.xml` file:

```xml
<service
    android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService"
android:exported="true">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>

<meta-data
android:name="CLEVERTAP_NOTIFICATION_ICON" 
android:value="ic_stat_red_star"/>

<service android:name="com.clevertap.android.sdk.CTNotificationIntentService" >
  <intent-filter>
    <action android:name="com.clevertap.PUSH_EVENT"/> 
  </intent-filter>
</service>
```

**Case 2**: When you send Push from different tools, such as Firebase.  
To enable this, perform the following steps:

1. Add the following code to your `AndroidManifest.xml` file:

```xml
<meta-data
  android:name="CLEVERTAP_NOTIFICATION_ICON" 
  android:value="ic_stat_red_star"/>
<!—Above meta data is optional -->

<service android:name="com.clevertap.android.sdk.CTNotificationIntentService" >
  <intent-filter>
    <action android:name="com.clevertap.PUSH_EVENT"/> 
  </intent-filter>
</service>
```

2. Add the following code to your `onNewToken` method: 

> 📘 Note
> 
> Use the `onTokenRefresh` method only if your firebase message delivery system version is 18.0 and above.

```kotlin
val fcmRegId = FirebaseInstanceId.getInstance().token
clevertapDefaultInstance?.pushFcmRegistrationId(fcmRegId, true)
```
```java
String fcmRegId = FirebaseInstanceId.getInstance().getToken(); 
clevertapDefaultInstance.pushFcmRegistrationId(fcmRegId,true);
```

3. Add the following code in your `MyFcmMessageListenerService` class file.

```kotlin
class MyFcmMessageListenerService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)
        try {
            if (message.data.isNotEmpty()) {
                val extras = Bundle()
                for (entry: Map.Entry<String?, String?> in message.data) {
                    extras.putString(entry.key, entry.value)
                }

                val info = CleverTapAPI.getNotificationInfo(extras)
                if (info.fromCleverTap) {
                    CleverTapAPI.createNotification(applicationContext, extras)
                } else {
                    // not from CleverTap handle yourself or pass to another provider
                }
            }
        } catch (t: Throwable) {
            Log.d("MYFCMLIST", "Error parsing FCM message", t)
        }
    }
}
```
```java
public class MyFcmMessageListenerService extends FirebaseMessagingService {
@Override
public void onMessageReceived(RemoteMessage message){
try {
  if (message.getData().size() > 0) {
        Bundle extras = new Bundle();
        for (Map.Entry<String, String> entry : message.getData().entrySet()) 
        {
            extras.putString(entry.getKey(), entry.getValue()); 
            }
        NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);
        if (info.fromCleverTap) 
        { 
          CleverTapAPI.createNotification(getApplicationContext(), extras);
        } else {
        // not from CleverTap handle yourself or pass to another provider
        } 
    }
  } catch (Throwable t) {
  Log.d("MYFCMLIST", "Error parsing FCM message", t);
  }
} 
}
```

4. Add the following code to create a Notification Channel for Android O and above:

```kotlin
CleverTapAPI.createNotificationChannel(applicationContext, "YourChannelId", "Your Channel Name", "Your Channel Description", NotificationManager.IMPORTANCE_MAX, true)
```
```java
CleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","Your Channel Name","Your Channel Description",NotificationManager.IMPORTANCE_MAX,true);
```

# Integrate In-App in Android

All the required methods are available out of the box in the Segment-CleverTap SDK.  
For more information about integrating In-App in Android, refer to [Android In-App](doc:android-in-app-notifications).

# Integrate App Inbox in Android

App Inbox allows you to send permanent content directly to your application from the CleverTap dashboard. Moreover, the inbox messages target individual segments such as other messaging channels. Your inbox can look different from another user's inbox; the possibilities are endless.

Add the following code to initialize the App Inbox and register callbacks:

```kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.clevertap.android.sdk.CTInboxListener
import com.clevertap.android.sdk.CTInboxStyleConfig
import com.clevertap.android.sdk.CleverTapAPI
import com.segment.analytics.Analytics
import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration

import java.util.ArrayList


class MainActivity : AppCompatActivity(), CTInboxListener {
    private var ctApiGlobalInstance: CleverTapAPI? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Analytics.Builder(applicationContext, SOURCE_ANDROID_KEY)
            .use(CleverTapIntegration.FACTORY)
            .build()
            .onIntegrationReady(
                "CleverTap",
                Analytics.Callback { ctApi: CleverTapAPI ->
                    // set instance as global for future use
                    ctApiGlobalInstance = ctApi

                    //Set the Notification Inbox Listener
                    ctApiGlobalInstance?.ctNotificationInboxListener = this

                    //Initialize the inbox and wait for callbacks on overridden methods
                    ctApiGlobalInstance?.initializeInbox()
                } as Analytics.Callback<CleverTapAPI>
            )
    }

    override fun inboxDidInitialize() {
        // add an on click to your inbox button to call showAppInbox() with custom or default styling
        yourInboxButton.setOnClickListener { v ->
            val tabs: ArrayList<String> = ArrayList()
            tabs.add("Promotions")
            tabs.add("Offers") //We support upto 2 tabs only. Additional tabs will be ignored
            val styleConfig = CTInboxStyleConfig()
            styleConfig.firstTabTitle = "First Tab"
            styleConfig.tabs = tabs //Do not use this if you don't want to use tabs
            styleConfig.tabBackgroundColor = "#FF0000"
            styleConfig.selectedTabIndicatorColor = "#0000FF"
            styleConfig.selectedTabColor = "#0000FF"
            styleConfig.unselectedTabColor = "#FFFFFF"
            styleConfig.backButtonColor = "#FF0000"
            styleConfig.navBarTitleColor = "#FF0000"
            styleConfig.navBarTitle = "MY INBOX"
            styleConfig.navBarColor = "#FFFFFF"
            styleConfig.inboxBackgroundColor = "#ADD8E6"
            if (ctApiGlobalInstance != null) {
                ctApiGlobalInstance!!.showAppInbox(styleConfig) //With Tabs
                //ctApiGlobalInstance.showAppInbox();//Opens Activity with default style configs
            }
        }
    }

    override fun inboxMessagesDidUpdate() {
        //Callback on Inbox Message update/delete/read (any activity)
    }
}
```
```java
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import com.clevertap.android.sdk.CTInboxListener;
import com.clevertap.android.sdk.CTInboxStyleConfig;
import com.clevertap.android.sdk.CleverTapAPI;
import com.segment.analytics.Analytics;
import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity implements CTInboxListener {

    private CleverTapAPI ctApiGlobalInstance = null;

    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        new Analytics
                .Builder(getApplicationContext(),SOURCE_ANDROID_KEY)
                .use(CleverTapIntegration.FACTORY)
                .build()
                .onIntegrationReady(
                        "CleverTap",
                        (Analytics.Callback<CleverTapAPI>) ctApi -> {
                            // set instance as global for future use
                            ctApiGlobalInstance = ctApi;

                            //Set the Notification Inbox Listener
                            ctApiGlobalInstance.setCTNotificationInboxListener(this);

                            //Initialize the inbox and wait for callbacks on overridden methods
                            ctApiGlobalInstance.initializeInbox();

                        }
                );
    }
    @Override
    public void inboxDidInitialize() {
        // add an on click to your inbox button to call showAppInbox() with custom or default styling
        yourInboxButton.setOnClickListener(v -> {
            ArrayList<String> tabs = new ArrayList<>();
            tabs.add("Promotions");
            tabs.add("Offers");//We support upto 2 tabs only. Additional tabs will be ignored

            CTInboxStyleConfig styleConfig = new CTInboxStyleConfig();
            styleConfig.setFirstTabTitle("First Tab");
            styleConfig.setTabs(tabs);//Do not use this if you don't want to use tabs
            styleConfig.setTabBackgroundColor("#FF0000");
            styleConfig.setSelectedTabIndicatorColor("#0000FF");
            styleConfig.setSelectedTabColor("#0000FF");
            styleConfig.setUnselectedTabColor("#FFFFFF");
            styleConfig.setBackButtonColor("#FF0000");
            styleConfig.setNavBarTitleColor("#FF0000");
            styleConfig.setNavBarTitle("MY INBOX");
            styleConfig.setNavBarColor("#FFFFFF");
            styleConfig.setInboxBackgroundColor("#ADD8E6");
            if (ctApiGlobalInstance != null) {
                ctApiGlobalInstance.showAppInbox(styleConfig); //With Tabs
                //ctApiGlobalInstance.showAppInbox();//Opens Activity with default style configs
            }
        });
    }
    @Override
    public void inboxMessagesDidUpdate() {
        //Callback on Inbox Message update/delete/read (any activity)
    }
}
```

For additional methods and App Inbox features, refer to the [App Inbox for Android](https://developer.clevertap.com/docs/android-app-inbox).

# Integrate Native Display in Android

CleverTap supports Native Display for CleverTap Android SDK version 3.6.2 and above.  
To integrate Native Display with your application:

1. Add the following code to register the Native Display Listener and get the NativeDisplayUnits using the `setDisplayUnitListener` method:

```kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.displayunits.DisplayUnitListener
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit
import com.segment.analytics.Analytics
import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration

class MainActivity : AppCompatActivity(), DisplayUnitListener {
    private var ctApiGlobalInstance: CleverTapAPI? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Analytics.Builder(applicationContext, SOURCE_ANDROID_KEY)
            .use(CleverTapIntegration.FACTORY)
            .build()
            .onIntegrationReady(
                "CleverTap",
                Analytics.Callback { ctApi: CleverTapAPI ->
                    // set instance as global for future use
                    ctApiGlobalInstance = ctApi

                    //set display unit listener
                    ctApiGlobalInstance?.setDisplayUnitListener(this)

                    // We have also provided a few utility methods to get Adunits corresponding to the last event raised.
                    ctApiGlobalInstance?.allDisplayUnits
                } as Analytics.Callback<CleverTapAPI>
            )
    }

    override fun onDisplayUnitsLoaded(units: ArrayList<CleverTapDisplayUnit>?) {
        // you will get display units here
        // implement your logic to create your display views using these Display Units here
        for (i in 0 until units!!.size) {
            val unit = units[i]
            prepareDisplayView(unit) //your function to create display unit
        }
    }
}
```
```java
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.displayunits.DisplayUnitListener;
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;
import com.segment.analytics.Analytics;
import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity implements DisplayUnitListener {

    private CleverTapAPI ctApiGlobalInstance = null;

    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        new Analytics
                .Builder(getApplicationContext(),SOURCE_ANDROID_KEY)
                .use(CleverTapIntegration.FACTORY)
                .build()
                .onIntegrationReady(
                        "CleverTap",
                        (Analytics.Callback<CleverTapAPI>) ctApi -> {
                            // set instance as global for future use
                            ctApiGlobalInstance = ctApi;

                            //set display unit listener
                            ctApiGlobalInstance.setDisplayUnitListener(this);

                           

                            // We have also provided a few utility methods to get Adunits corresponding to the last event raised.
                            ctApiGlobalInstance.getAllDisplayUnits();
                        }
                );
    }
    @Override
    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
        // you will get display units here
        // implement your logic to create your display views using these Display Units here
        for (int i = 0; i <units.size() ; i++) {
            CleverTapDisplayUnit unit = units.get(i);
            prepareDisplayView(unit); //your function to create display unit
        }
    }
}
```

Once the campaign for native display is triggered, you will receive the list of `CleverTapDisplayUnit` via the registered callback for all active Native Display campaigns. This `CleverTapDisplayUnit` is used to create Native Display views.

2. Add the following code to raise the user event for the Native Display campaign:

```kotlin
ctApiGlobalInstance?.pushEvent("Your Event Name")
```
```java
ctApiGlobalInstance.pushEvent("Your Event Name");
```

For additional methods and Native Display features, refer to [Native Display for Android](doc:android-native-display).

# Advanced Features

To add advanced features to your Android application, refer to [Android Advanced Settings](doc:android-advanced-features).
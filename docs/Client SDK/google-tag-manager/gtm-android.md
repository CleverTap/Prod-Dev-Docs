---
title: CleverTap GTM Android Integration
excerpt: Learn how to send Android app events to CleverTap via GTM
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

A Firebase account must be set up to use CleverTap Push notifications and other services.\
Google Tag Manager mainly interacts with the *Analytics* feature of firebase. When your app sends events to Firebase Analytics, those events can automatically trigger cloud functions defined in GTM. These cloud functions can further call some functions defined in your native app's code with the values from the said cloud function.

These functions, defined in the client's code, can be used to run CleverTap's SDK APIs.

A few integrations are required inside the native code. The triggers functions and the logic for passing data to native code/calling classes are configured on the GTM dashboard. There is no additional setup required in the Firebase console.

# Integration

To integrate GTM with CleverTap you need to:

1. [Configure CleverTap Dashboard with the Native Android App](doc:gtm-android#configure-clevertap-dashboard-with-native-android-app).
2. [Configure Firebase](doc:gtm-android#configure-firebase).
3. [Configure Google Tag Manager](doc:gtm-android#configure-gtm-dashboard).
4. [Set Up triggers and tags for Firebase Events in GTM](doc:gtm-android#set-up-triggers-and-tags-for-firebase-events-in-gtm).

## Configure CleverTap Dashboard with native Android App:

Integrate the [CleverTap Android SDK](doc:android-quickstart-guide) and ensure that you can create custom events.

## Configure Firebase

To enable Google Analytics for your app, perform the steps listed in the [Firebase Guide](https://firebase.google.com/docs/analytics/get-started?platform=android). Ensure that you can fire events via firebase analytics classes such as `firebaseAlayticsObj.logEvent("buttonClick", bundle);`.

There is no explicit configuration needed for Firebase to GTM.

## Configure GTM Dashboard

To configure the GTM dashboard:

1. Navigate to [https://tagmanager.google.com/](https://tagmanager.google.com/) and create a new account.

<Image title="Create New Account on GTM Dashboard" alt={2844} align="center" border={true} src="https://files.readme.io/0a7a2dd-GTM1.png">
  Create New Account on GTM Dashboard
</Image>

2. Select *Android* as the *Target platform* in the *Container Setup* section.

<Image title="Select Android in Container Setup" alt={2468} align="center" border={true} src="https://files.readme.io/5d31b30-GTM_2.png">
  Select Android in Container Setup
</Image>

3. Navigate to *Workspace* > *Latest version* and download the container JSON file after account creation.

<Image title="GTM Dashboard" alt={2584} align="center" border={true} src="https://files.readme.io/235fdf0-GTM_3.png">
  GTM Dashboard
</Image>

4. Add the dependency in your app's `build.gradle` file of your Native Android app:

```groovy
dependencies {
  // ...
  compile 'com.google.android.gms:play-services-tagmanager:18.0.1'
}
```

5. Place the downloaded JSON file from the previous step into the app > src > main assets > containers folder.
6. Create a class extending Custom Tag Receiver **with access to a static instance of clevertapApi**.

```kotlin
class GTMReciever:CustomTagProvider {
    
    companion object{
        var cleverTapAPIObj:CleverTapAPI? = null // make sure to initialise this instance in your Activity/Fragment/Application class
    }
    
    override fun execute(map: MutableMap<String, Any>) {
        val action = map.remove("actionType")
        when (action) {
            "customEvent" -> {
                val event = map.remove("eventName").toString()
                cleverTapAPIObj?.pushEvent(event,map)
            }
            "profileUpdate" -> cleverTapAPIObj?.pushProfile(map)
            else -> log("Got unknown action type: $action")
        }
    }
}

class MyApp : Application() {
    override fun onCreate() {
        //...
        GTMReciever.cleverTapAPIObj = CleverTapAPI.getDefaultInstance(this)
        //...
   }
}
```
```java Java
public final class GTMReciever implements CustomTagProvider {
    @Nullable
    private static CleverTapAPI cleverTapAPIObj;


    public void execute(@NotNull Map map) {
        String action = (String) map.remove("actionType");
        if (Objects.equals(action, "customEvent")) {
            String event = String.valueOf(map.remove("eventName"));
            if (cleverTapAPIObj != null) {
                cleverTapAPIObj.pushEvent(event, map);
            }
        } else if (Objects.equals(action, "profileUpdate")) {
            if (cleverTapAPIObj != null) {
                cleverTapAPIObj.pushProfile(map);
            }
        } else {
            Log.e("Error", "Got unknown action type: " + action);
        }

    }

    @Nullable
    public static CleverTapAPI getCleverTapAPIObj() {
        return cleverTapAPIObj;
    }

    public static void setCleverTapAPIObj(@Nullable CleverTapAPI api) {
        cleverTapAPIObj = api;
    }

}

//-------------------------------
public final class MyApp extends Application {
    public void onCreate() {
        super.onCreate();
        GTMReciever.setCleverTapAPIObj(CleverTapAPI.getDefaultInstance(this));
    }
}
```

7. Add some Firebase events as follows:

```kotlin
btn.setOnClickListener{
            val analytics = FirebaseAnalytics.getInstance(ctx)
            analytics.logEvent("fb_random_event", bundleOf("calling_time" to getRandomString()))
}
```
```java Java
btn.setOnClickListener(it -> {
            FirebaseAnalytics analytics = FirebaseAnalytics.getInstance(ctx);
            Bundle data = new Bundle();
            data.putString("calling_time", getRandomString());
            analytics.logEvent("fb_random_event", data);
        });
```

## Set Up triggers and tags for Firebase Events in GTM

1. Navigate to *Triggers* and create a new trigger from the GTM dashboard. 
2. Select *Custom* from the *Trigger Type* dropdown.  
3. Enter the event details under the *Details* section that you just fired via the code mentioned in the previous step.

<Image title="Trigger Configuration for Firebase Events in GTM" alt={2368} align="center" border={true} src="https://files.readme.io/6a5f433-GTM_4.png">
  Trigger Configuration for Firebase Events in GTM
</Image>

4. Navigate to *Tags* and create a new tag. 
5. Select *Function Call* as the *Tag Type* and add the path of the Custom Tag receiver class we previously defined. 

Here you can add information about all the events, parameters, and information you want to pass from the GTM Dashboard to the receiver. Also, under the trigger section, select the name of the previously created trigger 

<Image title="Select the Trigger" alt={2038} align="center" border={true} src="https://files.readme.io/6f15728-gtm_5.png">
  Select the Trigger
</Image>

6. Click **Save** and click  **Publish**. 

Your App is now ready to fire CleverTap events. The custom tag provider in our example uses this key to determine what action to take, and what event name to send to CleverTap when it receives data from Google Tag Manager.

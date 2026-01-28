---
title: Android TV Integration with Android SDK
excerpt: Learn how to integrate CleverTap SDK  for apps on Android TV.
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

The Android SDK for TV Operating System (OS) is similar to the Android SDK for Mobile. Most core components work similarly, such as views, activities, fragments, services, and more. However, some classes in core Android SDK are not supported for TV. Therefore, some features would not work out of the box. For example, `NotificationManager.notify()` does not create notifications. We use custom workarounds to use these features, such as creating a floating view and a cross button. It removes the floating view when pressed.

# Steps to Integrate

1. [Integrate CleverTap SDK, FCM Service in Gradle, and Manifest](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#integrate-clevertap-sdk-fcm-service-in-gradle-and-manifest)
2. [Initialize CleverTap SDK in the Application Class](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#initialize-clevertap-sdk-in-the-application-class)
3. [Create a User Profile](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#create-a-user-profile)
4. [Analytics](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#analytics)
5. [Add Custom FCM Class Code](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#add-custom-fcm-class-code)
6. [Create a Floating View ](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#create-a-floating-view)

## Integrate CleverTap SDK, FCM Service in Gradle, and Manifest

This section describes integrating CleverTap SDK FCM service in Gradle and Manifest.

* [Gradle Files](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#gradle-files)
* [Manifest Files](https://developer.clevertap.com/docs/android-tv-integration-with-android-sdk#manifest-files)

### Gradle Files

To integrate CleverTap SDK in the Gradle file, add the dependencies below in your application's `build.gradle` file:

```groovy
/* ============ project-root/build.gradle ============ */

//...
buildscript {
    dependencies {
        classpath 'com.google.gms:google-services:4.3.14'
    }
}
//...

/* ============ src/build.gradle ========== */

//...
plugins {
    //...
    id 'com.google.gms.google-services'
}
//...
dependencies {
    implementation "androidx.core:core-ktx:1.7.0"
    implementation "androidx.leanback:leanback:1.0.0"
    implementation "androidx.appcompat:appcompat:1.4.0"
    implementation "androidx.constraintlayout:constraintlayout:2.1.2"

    // google libs
    implementation platform("com.google.firebase:firebase-bom:25.10.0")
    implementation "com.google.firebase:firebase-analytics-ktx"
    implementation "com.google.firebase:firebase-messaging-ktx"
    implementation "com.google.android.gms:play-services-location:18.0.0" 
    implementation "com.google.android.exoplayer:exoplayer:2.15.1"
    implementation "com.google.android.material:material:1.4.0"

    // com.android libs
    implementation "com.android.installreferrer:installreferrer:2.2"
    

    // 3rd party libs
    implementation "com.github.bumptech.glide:glide:4.11.0"
    
    //CLEVERTAP Libs
    implementation "com.clevertap.android:clevertap-android-sdk:7.8.0"
    //implementation "com.clevertap.android:push-templates:2.2.0"
    //implementation "com.clevertap.android:clevertap-hms-sdk:1.5.1"
    //implementation "com.clevertap.android:clevertap-geofence-sdk:1.4.0"
    //implementation "com.clevertap.android:clevertap-xiaomi-sdk:1.4.0"
   

}

/* ============ src/google-services.json ========== */

        {
            /*add your app in a firebase project ,generate google-services.json, and add it in src folder */
        }

```

### Manifest Files

To add the CleverTap SDK in the Manifest file, use the following code snippet:

```xml
<!-- ============ src/main/AndroidManifest.xml ============ -->
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <!--  required by clevertap sdk  -->
    <uses-permission android:name="android.permission.INTERNET" />
    
    <!--  needed for notification view to float  -->
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

    <!-- ... -->
    
    <application android:name=".MainApp">
        <meta-data android:name="CLEVERTAP_ACCOUNT_ID" android:value="YOUR_ACCOUNT_ID" />
        <meta-data android:name="CLEVERTAP_TOKEN" android:value="YOUR_TOKEN" />
        <meta-data android:name="CLEVERTAP_REGION" android:value="YOUR_REGION"/>
    
        <!-- ... -->
        
        <!--  required by clevertap sdk  -->
        <service
            android:name="com.clevertap.ct_demo_androidtv.MyFcmMessageListenerService"
            android:exported="true">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
    </application>
</manifest>
```

## Initialize CleverTap SDK in the Application Class

Use the following code snippet to initialize CleverTap SDK in the Application class. It allows you to capture system events correctly.

```kotlin
class MainApp : Application() {

    var ctCoreApi: CleverTapAPI? = null

    override fun onCreate() {
        CleverTapAPI.setDebugLevel(com.clevertap.android.sdk.CleverTapAPI.LogLevel.VERBOSE)
        ActivityLifecycleCallback.register(this)

        super.onCreate()
        ctCoreApi = CleverTapAPI.getDefaultInstance(applicationContext)

        val importance = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) NotificationManager.IMPORTANCE_MAX else 5
        CleverTapAPI.createNotificationChannel(applicationContext, "id", "name", "description", importance, true)

    }
    /*...*/
}
```
```coffeescript Java
public class MainApp extends Application {

    private CleverTapAPI ctCoreApi;

    @Override
    public void onCreate() {
        super.onCreate();

        CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.VERBOSE);
        ActivityLifecycleCallback.register(this);

        ctCoreApi = CleverTapAPI.getDefaultInstance(getApplicationContext());

        int importance;
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            importance = NotificationManager.IMPORTANCE_HIGH; // IMPORTANCE_MAX is not available, use IMPORTANCE_HIGH
        } else {
            importance = NotificationManager.IMPORTANCE_DEFAULT;
        }

        CleverTapAPI.createNotificationChannel(
                getApplicationContext(),
                "id",
                "name",
                "description",
                importance,
                true
        );
    }
}

```

## Create a User Profile

To log in the user, use the following code:

```java
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");    // String
profileUpdate.put("Identity", 61026032);      // String or number
profileUpdate.put("Email", "jack@gmail.com"); // Email address of the user
profileUpdate.put("Phone", "+14155551234");   // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");             // Can be either M or F
profileUpdate.put("DOB", new Date());         // Date of Birth. Set the Date object to the appropriate value first
// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);        // Disable email notifications
profileUpdate.put("MSG-push", true);          // Enable push notifications
profileUpdate.put("MSG-sms", false);          // Disable SMS notifications
profileUpdate.put("MSG-whatsapp", true);      // Enable WhatsApp notifications
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings
String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

ctCoreApi.onUserLogin(profileUpdate);
```
```kotlin
val profileUpdate = HashMap<String, Any>()
profileUpdate["Name"] = "Jack Montana" // String
profileUpdate["Identity"] = 61026032 // String or number
profileUpdate["Email"] = "jack@gmail.com" // Email address of the user
profileUpdate["Phone"] = "+14155551234" // Phone (with the country code, starting with +)
profileUpdate["Gender"] = "M" // Can be either M or F
profileUpdate["DOB"] = Date() // Date of Birth. Set the Date object to the appropriate value first
profileUpdate["Photo"] = "www.foobar.com/image.jpeg" // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate["MSG-email"] = false // Disable email notifications
profileUpdate["MSG-push"] = true // Enable push notifications
profileUpdate["MSG-sms"] = false // Disable SMS notifications
profileUpdate["MSG-dndPhone"] = true // Opt out phone
profileUpdate["MSG-dndEmail"] = true // Opt out email
profileUpdate["MyStuff"] = arrayListOf("bag", "shoes") //ArrayList of Strings
profileUpdate["MyStuff"] = arrayOf("Jeans", "Perfume") //String Array
ctCoreApi?.onUserLogin(profileUpdate)
```

## Analytics

The following Android SDK methods can be used for analytics on Smart TVs.

### Custom Event

To raise a custom event, use the following code:

```java
ctCoreApi.pushEvent("Product viewed");
```
```kotlin
ctCoreApi?.pushEvent("Product viewed")
```

### Custom Event with Event Properties

To raise a custom event with event properties, use the following code:

```java
HashMap<String, Object> prodViewedAction = new HashMap<String, Object>();
prodViewedAction.put("Product Name", "Casio Chronograph Watch");
prodViewedAction.put("Category", "Mens Accessories");
prodViewedAction.put("Price", 59.99);
prodViewedAction.put("Date", new java.util.Date());

ctCoreApi.pushEvent("Product viewed", prodViewedAction);
```
```kotlin
val prodViewedAction = mapOf(
"Product Name" to "Casio Chronograph Watch",
"Category" to "Mens Accessories",
"Price" to 59.99,
"Date" to Date())
ctCoreApi?.pushEvent("Product viewed", prodViewedAction)
```

### Charged Event

To raise a _Charged_ event, use the following code:

```java
HashMap<String, Object> chargeDetails = new HashMap<String, Object>();
chargeDetails.put("Amount", 300);
chargeDetails.put("Payment Mode", "Credit card");
chargeDetails.put("Charged ID", 24052013);

HashMap<String, Object> item1 = new HashMap<String, Object>();
item1.put("Product category", "books");
item1.put("Book name", "The Millionaire next door");
item1.put("Quantity", 1);

HashMap<String, Object> item2 = new HashMap<String, Object>();
item2.put("Product category", "books");
item2.put("Book name", "Achieving inner zen");
item2.put("Quantity", 1);

HashMap<String, Object> item3 = new HashMap<String, Object>();
item3.put("Product category", "books");
item3.put("Book name", "Chuck it, let's do it");
item3.put("Quantity", 5);

ArrayList<HashMap<String, Object>> items = new ArrayList<HashMap<String, Object>>();
items.add(item1);
items.add(item2);
items.add(item3);

try {
    ctCoreApi.pushChargedEvent(chargeDetails, items);
} catch (InvalidEventNameException e) {
    // You have to specify the first parameter to push()
    // as CleverTapAPI.CHARGED_EVENT
}
```
```kotlin
fun recordPurchase(cleverTapAPI: CleverTapAPI){
        val charges = hashMapOf<String,Any>("Total Number Of Items" to 3, "Total Amount" to 400)         
        val items = arrayListOf(
                hashMapOf<String,Any>("Item name" to "Shoes", "Number of Items" to 1, "Amount" to 200),
                hashMapOf<String,Any>("Item name" to "Watch", "Number of Items" to 1, "Amount" to 100),
                hashMapOf<String,Any>("Item name" to "Biscuit", "Number of Items" to 1, "Amount" to 100),
        )
        ctCoreApi?.pushChargedEvent(charges,items)
}
```

## Add Custom FCM Class Code

To add a custom FCM class, add the following code:

```kotlin
class MyFcmMessageListenerService : FirebaseMessagingService() {
/*make sure this service is part of android manifest, as we did in step 1 */

    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)
        NotifUtil.floatingNotif(this,message)
    }
}
```

## Create a Floating View

To create a floating view every time FCM receives a notification, you must create a helper function that will:

1. Create a notification-like view by inflating an XML layout.
2. Set listeners on the view's buttons, extract data from RemoteMessage, and set it on the view's text areas.
3. Add it to the current window and position it at the bottom.

You can create an XML layout and use the `NotifUtil` utility to perform the above steps:

```kotlin
class NotifUtil {
    companion object{
        fun floatingNotif(context: Context, message: RemoteMessage){

            Handler(Looper.getMainLooper()).post {
                val windowManager = context.getSystemService(FirebaseMessagingService.WINDOW_SERVICE) as WindowManager
                val overlay = if (Build.VERSION.SDK_INT < Build.VERSION_CODES.O) WindowManager.LayoutParams.TYPE_PHONE else   WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY
                val params = WindowManager.LayoutParams(WindowManager.LayoutParams.WRAP_CONTENT, WindowManager.LayoutParams.WRAP_CONTENT, overlay, WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE, PixelFormat.TRANSLUCENT).also {
                    it.gravity = Gravity.START or Gravity.BOTTOM
                }
                
                val inflater = context.getSystemService(FirebaseMessagingService.LAYOUT_INFLATER_SERVICE) as LayoutInflater
                val floatingView: View = inflater.inflate(R.layout.floating_view, null)
                floatingView.setOnClickListener {
                    windowManager.removeView(floatingView)
                    val intent = Intent(it.context,MainActivity::class.java)
                    intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK
                    it.context.startActivity(intent)

                    CleverTapAPI.getDefaultInstance(context).pushNotificationClickedEvent(message.data.toBundle()) // <-- important  : do this to record notification event
                }

                floatingView.findViewById<ImageButton>(R.id.btCross).setOnClickListener { windowManager.removeView(floatingView) }
                //todo takeout data from notif and add it on other views


                windowManager.addView(floatingView, params)
            }


        }

    }
}
```

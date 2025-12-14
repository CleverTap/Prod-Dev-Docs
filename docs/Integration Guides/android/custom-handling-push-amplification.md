---
title: Custom Push Amplification Handling
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
1. In your custom FCM Receiver class, add the following code snippet when you receive the RemoteMessage object from Firebase. This method allows you to ensure that duplicate Push notification is not rendered on your device via Push Amplification.

```java JAVA
if (message.getData().size() > 0) {
                Bundle extras = new Bundle();
                Iterator var = message.getData().entrySet().iterator();

                while(var.hasNext()) {
                    Map.Entry entry = (Map.Entry)var.next();
                    extras.putString((String)entry.getKey(), (String)entry.getValue());
                }
                    CleverTapAPI.processPushNotification(getApplicationContext(),extras);
                }
            }
```
```kotlin Kotlin
if (message.getData().size() > 0)
{
  val extras = Bundle()
  val `var` = message.getData().entrySet().iterator()
  while (`var`.hasNext())
  {
    val entry = `var`.next() as Map.Entry
    extras.putString(entry.getKey() as String, entry.getValue() as String)
  }
  CleverTapAPI.processPushNotification(getApplicationContext(), extras)
}
```

2. In your Application class, implement the CTPushAmpListener interface.

```java JAVA
public class MyApplication extends Application implements CTPushAmpListener {

// your application class code
}
```
```kotlin Kotlin
class MyApplication:Application(), CTPushAmpListener// your application class code
```

3. In the onCreate() method of your Application class, create an instance of CleverTapAPI and set the `CTPushAmpListener` as following:

```java JAVA
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
cleverTapAPI.setCTPushAmpListener(this);
```
```kotlin Kotlin
val cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext())
cleverTapAPI.setCTPushAmpListener(this)
```

4. After the listener is set, whenever a Push Notification is sent to the device via Push Amplification, the app will receive the payload in the onPushPayloadReceived method. Implement this method as follows and ensure that your rendering logic of the Push Notification is written here:

```java JAVA
@Override
public void onPushPayloadReceived(Bundle bundle) {
    //write push notification rendering logic here
}
```
```kotlin Kotlin
fun onPushPayloadReceived(bundle:Bundle) {
  //write push notification rendering logic here
}
```

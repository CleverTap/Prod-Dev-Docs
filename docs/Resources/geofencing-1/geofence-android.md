---
title: Geofence Android SDK
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
CleverTap Android Geofence SDK provides geofencing capabilities to CleverTap Android SDK by using the Play Services Location library. If you haven't already configured your project for Play Services, follow the instructions [here](https://developers.google.com/android/guides/google-services-plugin).
[block:api-header]
{
  "title": "Installation"
}
[/block]
Add the following dependencies to the `build.gradle`
[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.clevertap.android:clevertap-geofence-sdk:1.0.1'\nimplementation 'com.clevertap.android:clevertap-android-sdk:4.0.0'\nimplementation 'com.google.android.gms:play-services-location:17.0.0'\nimplementation 'androidx.work:work-runtime:2.3.4' // required for FETCH_LAST_LOCATION_PERIODIC\nimplementation 'androidx.concurrent:concurrent-futures:1.0.0' // required for FETCH_LAST_LOCATION_PERIODIC",
      "language": "groovy"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Permissions"
}
[/block]
In order to start using geofence in your app, the app needs the following permissions in `AndroidManifest.xml` which was added by the SDK.
[block:code]
{
  "codes": [
    {
      "code": "<uses-permission android:name=\"android.permission.ACCESS_FINE_LOCATION\" />\n<uses-permission android:name=\"android.permission.ACCESS_BACKGROUND_LOCATION\" />\n<uses-permission android:name=\"android.permission.WAKE_LOCK\" />\n<uses-permission android:name=\"android.permission.RECEIVE_BOOT_COMPLETED\"/>",
      "language": "xml"
    }
  ]
}
[/block]
The SDK initialization app needs to prompt users to grant the following permissions at runtime.
[block:code]
{
  "codes": [
    {
      "code": "<uses-permission android:name=\"android.permission.ACCESS_FINE_LOCATION\" />\n<uses-permission android:name=\"android.permission.ACCESS_BACKGROUND_LOCATION\" /> \n(Required only when requesting background location access on Android 10 (API level 29))",
      "language": "xml",
      "name": null
    }
  ]
}
[/block]
Since users can revoke permissions at any time from the app settings screen, your app needs to check that it has the permissions it needs every time it runs.

Refer to [Permissions](https://developer.android.com/preview/features/runtime-permissions.html) and [Location Permissions](https://developer.android.com/training/location/permissions#request-location-access-runtime) for more details.
[block:api-header]
{
  "title": "Initialization"
}
[/block]
`CTGeofenceAPI` needs an object of `CTGeofenceSettings` and the object of `CleverTapAPI` to be initialized in the following manner:
[block:code]
{
  "codes": [
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext()).init(ctGeofenceSettings,cleverTapAPI);",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext()).init(ctGeofenceSettings, cleverTapAPI)",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
`CTGeofenceSettings` object can be created in the following way:
[block:code]
{
  "codes": [
    {
      "code": "CTGeofenceSettings ctGeofenceSettings = new CTGeofenceSettings.Builder()\n                .enableBackgroundLocationUpdates(bgLocation)//boolean to enable background location updates\n                .setLogLevel(logLevel)//Log Level\n                .setLocationAccuracy(accuracy)//byte value for Location Accuracy\n                .setLocationFetchMode(fetchMode)//byte value for Fetch Mode\n                .setGeofenceMonitoringCount(geofenceCount)//int value for number of Geofences CleverTap can monitor\n                .setInterval(interval)//long value for interval in milliseconds\n                .setFastestInterval(fastestInterval)//long value for fastest interval in milliseconds\n                .setSmallestDisplacement(displacement)//float value for smallest Displacement in meters\n               .build();",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "var ctGeofenceSettings = CTGeofenceSettings.Builder()\n.enableBackgroundLocationUpdates(bgLocation)//boolean to enable background location updates\n.setLogLevel(logLevel)//Log Level\n.setLocationAccuracy(accuracy)//byte value for Location Accuracy\n.setLocationFetchMode(fetchMode)//byte value for Fetch Mode\n.setGeofenceMonitoringCount(geofenceCount)//int value for number of Geofences CleverTap can monitor\n.setInterval(interval)//long value for interval in milliseconds\n.setFastestInterval(fastestInterval)//long value for fastest interval in milliseconds\n.setSmallestDisplacement(displacement)//float value for smallest Displacement in meters\n.build()",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "* CleverTapAPI needs to be initialized before the CTGeofenceAPI object is created because the object of CleverTapAPI is required to initialize the CleverTap Android Geofence SDK.\n* Runtime location permissions needed for SDK to work.\n* CTGeofenceAPI is automatically activated once the init() method has been called.\n* CleverTap Android Geofence SDK raises the “Geofence Cluster Entered” and “Geofence Cluster Exited” events automatically. The app cannot raise these methods manually.",
  "title": "Note:"
}
[/block]

[block:api-header]
{
  "title": "Setting Parameters"
}
[/block]
Detailed parameter information is available [here](https://github.com/CleverTap/clevertap-geofence-android/blob/feature/geofence/SDK-264/docs/Settings.md).
[block:api-header]
{
  "title": "Trigger Location"
}
[/block]
This method fetches the last known location from OS (can be null) and delivers it to the app through `CTLocationUpdatesListener`. 

Detailed trigger location information is available [here](https://github.com/CleverTap/clevertap-geofence-android/blob/feature/geofence/SDK-264/docs/TriggerLocation.md).
[block:code]
{
  "codes": [
    {
      "code": "try {\n    CTGeofenceAPI.getInstance(getApplicationContext()).triggerLocation();\n} catch (IllegalStateException e){\n    // thrown when this method is called before geofence SDK initialization\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "try\n{\n  CTGeofenceAPI.getInstance(getApplicationContext()).triggerLocation()\n}\ncatch (e:IllegalStateException) {\n  // thrown when this method is called before geofence SDK initialization\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Callback/Listeners"
}
[/block]
CleverTap Android Geofence SDK provides three callbacks or listeners on the main thread to the app for more control to the developers.

## OnGeofenceApiInitializedListener

CleverTap Android Geofence SDK provides a callback on the main thread to notify that the CTGeofenceAPI class has been initialized. The callback can be used in the following way: 
[block:code]
{
  "codes": [
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext())\n       .setOnGeofenceApiInitializedListener(new CTGeofenceAPI.OnGeofenceApiInitializedListener() {\n           @Override\n           public void OnGeofenceApiInitialized() {\n               //App is notified on the main thread that CTGeofenceAPI is initialized\n           }\n       });",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext())\n.setOnGeofenceApiInitializedListener(object:CTGeofenceAPI.OnGeofenceApiInitializedListener() {\n  fun OnGeofenceApiInitialized() {\n    //App is notified on the main thread that CTGeofenceAPI is initialized\n  }\n})",
      "language": "text",
      "name": "Kotlin"
    }
  ]
}
[/block]
## CTGeofenceEventsListener

CleverTap Android Geofence SDK provides a callback on the main thread to the app when the user enters or exits a Geofence. The callback can be used in the following way:
[block:code]
{
  "codes": [
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext())\n       .setCtGeofenceEventsListener(new CTGeofenceEventsListener() {\n           @Override\n           public void onGeofenceEnteredEvent(JSONObject jsonObject) {\n                //Callback on the main thread when the user enters Geofence with info in jsonObject             \n           }\n\n           @Override\n           public void onGeofenceExitedEvent(JSONObject jsonObject) {\n               //Callback on the main thread when user exits Geofence with info in jsonObject             \n           }\n       });",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext())\n.setCtGeofenceEventsListener(object:CTGeofenceEventsListener() {\n  fun onGeofenceEnteredEvent(jsonObject:JSONObject) {\n    //Callback on the main thread when the user enters Geofence with info in jsonObject \n  }\n  fun onGeofenceExitedEvent(jsonObject:JSONObject) {\n    //Callback on the main thread when user exits Geofence with info in jsonObject \n  }\n})",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
Structure of JSON Object on Geofence Entered/Exited event :
[block:code]
{
  "codes": [
    {
      "code": "{\n      \"id\" : 500043 //geofenceUniqueID,\n      \"gcId\" : 1 //geofenceClusterId,\n      \"gcName\" : \"geofenceClusterName\",\n      \"lat\" : 19.229493674459807 //geofence Latitude,\n      \"lng\" : 72.82329440116882 //geofence Longitude,\n      \"r\" : 200 //geofenceRadiusInMeters,\n      \"triggered_lat\" : 19.229493674459807 //geofenceTriggeredLatitude,\n      \"triggered_lng\" : 72.82329440116882 //geofenceTriggeredLongitude\n   }",
      "language": "json"
    }
  ]
}
[/block]
 ## CTLocationUpdatesListener

CleverTap Android Geofence SDK provides a callback on the main thread to the app when Android OS provides a location update to the SDK. The callback can be used in the following way:
[block:code]
{
  "codes": [
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext())\n       .setCtLocationUpdatesListener(new CTLocationUpdatesListener() {\n           @Override\n           public void onLocationUpdates(Location location) {\n              //New location on the main thread as provided by the Android OS\n           }\n       });",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext())\n.setCtLocationUpdatesListener(object:CTLocationUpdatesListener() {\n  fun onLocationUpdates(location:Location) {\n    //New location on the main thread as provided by the Android OS\n  }\n})",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Deactivation"
}
[/block]
If you want to deactivate the CleverTap Android Geofence SDK, you can do so in the following way :
[block:code]
{
  "codes": [
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext()).deactivate();",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CTGeofenceAPI.getInstance(getApplicationContext()).deactivate()",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
**Note:** Deactivation removes all callbacks/listeners as well. 
[block:api-header]
{
  "title": "Example Usage"
}
[/block]
A [demo application](https://github.com/CleverTap/clevertap-android-sdk/tree/master/sample) showing geofence SDK integration.
[block:api-header]
{
  "title": "FAQ"
}
[/block]
FAQs are available [here](https://github.com/CleverTap/clevertap-android-sdk/tree/master/docs/FAQ.md). 
[block:api-header]
{
  "title": "Questions"
}
[/block]
If your question is not found in the FAQs and you have other questions or concerns, you can reach out to the CleverTap support team by raising an issue from the CleverTap dashboard.
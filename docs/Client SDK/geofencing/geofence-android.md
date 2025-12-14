---
title: Android Geofence SDK
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Overview

CleverTap Android Geofence SDK provides geofencing capabilities to CleverTap Android SDK by using the Play Services Location library. If you haven't already configured your project for Play Services, follow the instructions [here](https://developers.google.com/android/guides/google-services-plugin).

## Installation

Add the following dependencies to the `build.gradle`

```groovy
implementation 'com.clevertap.android:clevertap-geofence-sdk:1.4.0'
implementation 'com.clevertap.android:clevertap-android-sdk:7.5.0'
implementation 'com.google.android.gms:play-services-location:21.0.0'
implementation 'androidx.work:work-runtime:2.7.1' // required for FETCH_LAST_LOCATION_PERIODIC
implementation 'androidx.concurrent:concurrent-futures:1.1.0' // required for FETCH_LAST_LOCATION_PERIODIC
```

## Permissions

In order to start using geofence in your app, the app needs the following permissions in `AndroidManifest.xml` which was added by the SDK.

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

The SDK initialization app needs to prompt users to grant the following permissions at runtime.

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" /> 
(Required only when requesting background location access on Android 10 (API level 29))
```

Since users can revoke permissions at any time from the app settings screen, your app needs to check that it has the permissions it needs every time it runs.

Refer to [Permissions](https://developer.android.com/preview/features/runtime-permissions.html) and [Location Permissions](https://developer.android.com/training/location/permissions#request-location-access-runtime) for more details.

## Initialization

`CTGeofenceAPI` needs an object of `CTGeofenceSettings` and the object of `CleverTapAPI` to be initialized in the following manner:

```java JAVA
CTGeofenceAPI.getInstance(getApplicationContext()).init(ctGeofenceSettings,cleverTapAPI);
```
```kotlin Kotlin
CTGeofenceAPI.getInstance(getApplicationContext()).init(ctGeofenceSettings, cleverTapAPI)
```

`CTGeofenceSettings` object can be created in the following way:

```java JAVA
CTGeofenceSettings ctGeofenceSettings = new CTGeofenceSettings.Builder()
                .enableBackgroundLocationUpdates(bgLocation)//boolean to enable background location updates
                .setLogLevel(logLevel)//Log Level
                .setLocationAccuracy(accuracy)//byte value for Location Accuracy
                .setLocationFetchMode(fetchMode)//byte value for Fetch Mode
                .setGeofenceMonitoringCount(geofenceCount)//int value for number of Geofences CleverTap can monitor
                .setInterval(interval)//long value for interval in milliseconds
                .setFastestInterval(fastestInterval)//long value for fastest interval in milliseconds
                .setSmallestDisplacement(displacement)//float value for smallest Displacement in meters
                .setGeofenceNotificationResponsiveness(geofenceNotificationResponsiveness)// int value for geofence notification responsiveness in milliseconds
                .build();
```
```kotlin Kotlin
var ctGeofenceSettings = CTGeofenceSettings.Builder()
.enableBackgroundLocationUpdates(bgLocation)//boolean to enable background location updates
.setLogLevel(logLevel)//Log Level
.setLocationAccuracy(accuracy)//byte value for Location Accuracy
.setLocationFetchMode(fetchMode)//byte value for Fetch Mode
.setGeofenceMonitoringCount(geofenceCount)//int value for number of Geofences CleverTap can monitor
.setInterval(interval)//long value for interval in milliseconds
.setFastestInterval(fastestInterval)//long value for fastest interval in milliseconds
.setSmallestDisplacement(displacement)//float value for smallest Displacement in meters
.setGeofenceNotificationResponsiveness(geofenceNotificationResponsiveness)// int value for geofence notification responsiveness in milliseconds
.build()
```

> 📘 Note:
>
> * CleverTapAPI needs to be initialized before the CTGeofenceAPI object is created because the object of CleverTapAPI is required to initialize the CleverTap Android Geofence SDK.
> * Runtime location permissions needed for SDK to work.
> * CTGeofenceAPI is automatically activated once the init() method has been called.
> * CleverTap Android Geofence SDK raises the “Geofence Cluster Entered” and “Geofence Cluster Exited” events automatically. The app cannot raise these methods manually.

## Setting Parameters

Detailed parameter information is available on our Github page  [Settings Parameters](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTGEOFENCE.md).

## Trigger Location

This method fetches the last known location from OS (can be null) and delivers it to the app through `CTLocationUpdatesListener`. 

Detailed trigger location information is available on our Github page [Android Trigger Location](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/TriggerLocation.md).

```java JAVA
try {
    CTGeofenceAPI.getInstance(getApplicationContext()).triggerLocation();
} catch (IllegalStateException e){
    // thrown when this method is called before geofence SDK initialization
}
```
```kotlin Kotlin
try
{
  CTGeofenceAPI.getInstance(getApplicationContext()).triggerLocation()
}
catch (e:IllegalStateException) {
  // thrown when this method is called before geofence SDK initialization
}
```

## Callback/Listeners

CleverTap Android Geofence SDK provides three callbacks or listeners on the main thread to the app for more control to the developers.

## OnGeofenceApiInitializedListener

CleverTap Android Geofence SDK provides a callback on the main thread to notify that the CTGeofenceAPI class has been initialized. The callback can be used in the following way: 

```java JAVA
CTGeofenceAPI.getInstance(getApplicationContext())
       .setOnGeofenceApiInitializedListener(new CTGeofenceAPI.OnGeofenceApiInitializedListener() {
           @Override
           public void OnGeofenceApiInitialized() {
               //App is notified on the main thread that CTGeofenceAPI is initialized
           }
       });
```
```text Kotlin
CTGeofenceAPI.getInstance(getApplicationContext())
.setOnGeofenceApiInitializedListener(object:CTGeofenceAPI.OnGeofenceApiInitializedListener() {
  fun OnGeofenceApiInitialized() {
    //App is notified on the main thread that CTGeofenceAPI is initialized
  }
})
```

## CTGeofenceEventsListener

CleverTap Android Geofence SDK provides a callback on the main thread to the app when the user enters or exits a Geofence. The callback can be used in the following way:

```java JAVA
CTGeofenceAPI.getInstance(getApplicationContext())
       .setCtGeofenceEventsListener(new CTGeofenceEventsListener() {
           @Override
           public void onGeofenceEnteredEvent(JSONObject jsonObject) {
                //Callback on the main thread when the user enters Geofence with info in jsonObject             
           }

           @Override
           public void onGeofenceExitedEvent(JSONObject jsonObject) {
               //Callback on the main thread when user exits Geofence with info in jsonObject             
           }
       });
```
```kotlin Kotlin
CTGeofenceAPI.getInstance(getApplicationContext())
.setCtGeofenceEventsListener(object:CTGeofenceEventsListener() {
  fun onGeofenceEnteredEvent(jsonObject:JSONObject) {
    //Callback on the main thread when the user enters Geofence with info in jsonObject 
  }
  fun onGeofenceExitedEvent(jsonObject:JSONObject) {
    //Callback on the main thread when user exits Geofence with info in jsonObject 
  }
})
```

Structure of JSON Object on Geofence Entered/Exited event :

```json
{
      "id" : 500043 //geofenceUniqueID,
      "gcId" : 1 //geofenceClusterId,
      "gcName" : "geofenceClusterName",
      "lat" : 19.229493674459807 //geofence Latitude,
      "lng" : 72.82329440116882 //geofence Longitude,
      "r" : 200 //geofenceRadiusInMeters,
      "triggered_lat" : 19.229493674459807 //geofenceTriggeredLatitude,
      "triggered_lng" : 72.82329440116882 //geofenceTriggeredLongitude
   }
```

## CTLocationUpdatesListener

CleverTap Android Geofence SDK provides a callback on the main thread to the app when Android OS provides a location update to the SDK. The callback can be used in the following way:

```java JAVA
CTGeofenceAPI.getInstance(getApplicationContext())
       .setCtLocationUpdatesListener(new CTLocationUpdatesListener() {
           @Override
           public void onLocationUpdates(Location location) {
              //New location on the main thread as provided by the Android OS
           }
       });
```
```kotlin Kotlin
CTGeofenceAPI.getInstance(getApplicationContext())
.setCtLocationUpdatesListener(object:CTLocationUpdatesListener() {
  fun onLocationUpdates(location:Location) {
    //New location on the main thread as provided by the Android OS
  }
})
```

## Deactivation

If you want to deactivate the CleverTap Android Geofence SDK, you can do so in the following way :

```java JAVA
CTGeofenceAPI.getInstance(getApplicationContext()).deactivate();
```
```kotlin Kotlin
CTGeofenceAPI.getInstance(getApplicationContext()).deactivate()
```

**Note:** Deactivation removes all callbacks/listeners as well. 

## Example Usage

A [demo application](https://github.com/CleverTap/clevertap-android-sdk/tree/master/sample) showing geofence SDK integration.

## FAQ

FAQs are available [here](https://github.com/CleverTap/clevertap-android-sdk/tree/master/docs/FAQ.md). 

## Questions

If your question is not found in the FAQs and you have other questions or concerns, you can reach out to the CleverTap support team by raising an issue from the CleverTap dashboard.

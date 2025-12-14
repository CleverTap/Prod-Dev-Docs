---
title: Android Native Display
excerpt: Learn how to integrate and test Native Display.
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

Native Display helps to display content natively within your app without interrupting the user. It also provides the ability to change the content of your app dynamically and deliver relevant and contextual content to your users.

We support Native Display for CleverTap Android SDK version 3.6.2 and above. Follow the steps to integrate Native Display with your App.

> 📘 Support for Linked Content in Native Display
>
> Starting with Android SDK version 7.5.0 and above, Native Display units support [Linked Content](https://docs.clevertap.com/docs/linked-content), enabling richer, personalized In-App experiences with dynamic data.

## Register the Native Display Listener

The Activity class must implement DisplayUnitListener, on which you will show the NativeDisplayUnits. Register the activity to the Clevertap’s instance using the setDisplayUnitListener method.

```kotlin Kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.displayunits.DisplayUnitListener
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit
import java.util.ArrayList

class MainActivity: AppCompatActivity(), DisplayUnitListener {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this@MainActivity)

        cleverTapDefaultInstance?.apply {
            setDisplayUnitListener(this@MainActivity)
        }
    }
    override fun onDisplayUnitsLoaded(units: ArrayList<CleverTapDisplayUnit>?) {
        // you will get display units here
    }
}
```
```java JAVA
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.displayunits.DisplayUnitListener;
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;
import java.util.ArrayList;
public class MainActivity extends AppCompatActivity implements DisplayUnitListener {
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this);
    }
    @Override
    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
        // you will get display units here
    }
}
```

## Get the NativeDisplayUnits

You can raise the event for the Native Display Campaign.

```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).pushEvent("Your Event Name")
```
```java JAVA
CleverTapAPI.getDefaultInstance(this).pushEvent("Your Event Name");
```

You receive the list of CleverTapDisplayUnits via the registered callback for all active Native Display campaigns. Implement your logic to create Native Display views using these CleverTapDisplayUnits.

```kotlin Kotlin
override fun onDisplayUnitsLoaded(units:ArrayList<CleverTapDisplayUnit>) {
  // you will get display units here
  // implement your logic to create your display views using these Display Units here
  for (i in 0 until units.size())
  {
    val unit = units.get(i)
    prepareDisplayView(unit)
  }
}
```
```java JAVA
@Override
public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
    // you will get display units here
    // implement your logic to create your display views using these Display Units here
    for (int i = 0; i <units.size() ; i++) {
        CleverTapDisplayUnit unit = units.get(i);
        prepareDisplayView(unit);
    }
}
```

Refer to our model classes CleverTapDisplayUnit and CleverTapDisplayUnitContent for creating the Native Display view.

```java CleverTapDisplayUnit
/**
 * This model class holds the data of an individual Display Unit.
 */
public class CleverTapDisplayUnit implements Parcelable {
    /**
     * Display unit identifier
     */
    private String unitID;
    /**
     * Display Type Could be (banner,carousel,custom key value etc.)
     */
    private CTDisplayUnitType type;
    /**
     * Background Color
     */
    private String bgColor;
    /**
     * List of Display Content Items
     */
    private ArrayList<CleverTapDisplayUnitContent> contents;
    /**
     * Custom Key Value Pairs
     */
    private HashMap<String, String> customExtras;
    /**
     * Getter for the unitId of the Display Unit
     *
     * @return String
     */
    public String getUnitID() {
        return unitID;
    }
    /**
     * Getter for the Key Value pairs of the Display Unit
     *
     * @return HashMap<String, String>
     */
    public HashMap<String, String> getCustomExtras() {
        return customExtras;
    }
    /**
     * Getter for the hex-value background color of the Display Unit e.g. #000000
     *
     * @return String
     */
    public String getBgColor() {
        return bgColor;
    }
    /**
     * Getter for the DisplayUnitType of the Display Unit, Refer{@link CTDisplayUnitType}
     *
     * @return CTDisplayUnitType
     */
    public CTDisplayUnitType getType() {
        return type;
    }
    /**
     * Getter for the list of Content Display Unit Items.
     *
     * @return ArrayList<CleverTapDisplayUnitContent>
     */
    public ArrayList<CleverTapDisplayUnitContent> getContents() {
        return contents;
    }
}
```
```java CleverTapDisplayUnitContent
/**
 * Content class for holding Display Unit Content Data
 */
public class CleverTapDisplayUnitContent implements Parcelable {
    private String title;
    private String titleColor;
    private String message;
    private String messageColor;
    private String media;
    private String contentType;
    private String posterUrl;
    private String actionUrl;
    private String icon;
    /**
     * Getter for the title section of the Display Unit Content
     * @return String
     */
    public String getTitle() {
        return title;
    }
    /**
     * Getter for the message section of the Display Unit Content
     * @return String
     */
    public String getMessage() {
        return message;
    }
    /**
     * Getter for the media URL of the Display Unit Content
     * @return String
     */
    public String getMedia() {
        return media;
    }
    /**
     * Getter for the action URL of the body of the Display Unit Content
     * @return String
     */
    public String getActionUrl() {
        return actionUrl;
    }
    /**
     * Getter for the URL as String for the icon in case of Icon Message template
     * @return String
     */
    public String getIcon() {
        return icon;
    }
    /**
     * Getter for the hex-code value of the title color e.g. #000000
     * @return String
     */
    public String getTitleColor() {
        return titleColor;
    }
    /**
     * Getter for the hex-code value of the message color e.g. #000000
     * @return String
     */
    public String getMessageColor() {
        return messageColor;
    }
    /**
     * Getter for the URL for the thumbnail of the video
     * @return String
     */
    public String getPosterUrl() {
        return posterUrl;
    }
    /**
     * Getter for the content type of the media(image/gif/audio/video etc.)
     *
     * Refer{@link #mediaIsImage()}, {@link #mediaIsGIF()},
     *      {@link #mediaIsAudio()} ,{@link #mediaIsVideo()}
     * @return String
     */
    public String getContentType() {
        return contentType;
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is an image.
     *
     * @return boolean - | true, if the media type is image
     *                   | false, if the media type is not an image
     */
    public boolean mediaIsImage() {
        return contentType != null && this.media != null && contentType.startsWith("image") && !contentType.equals("image/gif");
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is a GIF.
     *
     * @return boolean - | true, if the media type is GIF
     *                   | false, if the media type is not a GIF
     */
    public boolean mediaIsGIF() {
        return contentType != null && this.media != null && contentType.equals("image/gif");
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is a video.
     *
     * @return boolean - | true, if the media type is video
     *                   | false, if the media type is not a video
     */
    public boolean mediaIsVideo() {
        return contentType != null && this.media != null && contentType.startsWith("video");
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is an audio.
     *
     * @return boolean - | true, if the media type is audio
     *                   | false, if the media type is not an audio
     */
    public boolean mediaIsAudio() {
        return contentType != null && this.media != null && contentType.startsWith("audio");
    }
}
```

We have also provided a few utility methods to get Adunits corresponding to the last event raised.

```java Getting all the Display Units
/**
 * Getter for retrieving all the Display Units.
 *
 * @return ArrayList<CleverTapDisplayUnit> - could be null, if there is no Display Unit campaigns
 */
@Nullable
public ArrayList<CleverTapDisplayUnit> getAllDisplayUnits();
```

```java Getting Display Unit using unitID
/**
 * Getter for retrieving Display Unit using the unitID
 *
 * @param unitID - unitID of the Display Unit {@link CleverTapDisplayUnit#getUnitID()}
 * @return CleverTapDisplayUnit - could be null, if there is no Display Unit campaign with the identifier
 */
@Nullable
public CleverTapDisplayUnit getDisplayUnitForId(String unitID);
```

## Raise Events for Tracking

You can raise the following events for tracking:

### Notification Viewed Event

You can raise the Notification Viewed event using the `pushDisplayUnitViewedEventForID` method whenever the user sees the Native Display. You must provide the unitID of the CleverTapDisplayUnit corresponding to the Native Display.

```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitViewedEventForID(adUnit.getUnitID())
```
```java JAVA
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitViewedEventForID(adUnit.getUnitID());
```

### Notification Clicked Event

You can raise the \_Notification Clicked \_event using the `pushDisplayUnitClickedEventForID` method whenever the user clicks the Native Display.  You must provide the unitID of the CleverTapDisplayUnit corresponding to the Native Display.

```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitClickedEventForID(adUnit.getUnitID())
```
```java JAVA
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitClickedEventForID(adUnit.getUnitID();;
```

> 📘 Custom Handling
>
> The events that you raise are accepted automatically by the CleverTap dashboard.\
> For example, you have implemented the Native Display using RecyclerView, and you raise a Notification Viewed event whenever the Native Display appears on the browser's viewport. If the users scroll through the app multiple times, multiple Notification Viewed events are raised. 
>
> In this case, you might not want to raise multiple events. You can handle this scenario using the android framework callbacks.

## Get Native Display Units using Send Test

The CleverTapDisplayUnit is delivered via a Push notification in the Send Test flow. As soon we receive the notification, we parse the notification payload containing the CleverTapDisplayUnit and keep them in memory.

You can get the CleverTapDisplayUnit payload by either of the following methods:

### Method 1: By getting all the Display Units

```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).getAllDisplayUnits()
```
```java JAVA
CleverTapAPI.getDefaultInstance(this).getAllDisplayUnits();
```

OR

### Method 2: Register the DisplayUnitListener in the Application Class

You will receive the CleverTapDisplayUnit.

```kotlin Kotlin
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.displayunits.DisplayUnitListener
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit
import java.util.ArrayList

class MyApplication:Application(), DisplayUnitListener {
  fun onCreate() {
    super.onCreate()
    CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this)
  }
  override fun onDisplayUnitsLoaded(units:ArrayList<CleverTapDisplayUnit>) {
    // you will get send test display units here
  }
}
```
```java JAVA
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.displayunits.DisplayUnitListener;
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;
import java.util.ArrayList;

public class MyApplication extends Application implements DisplayUnitListener {
    @Override
    public void onCreate(){
        super.onCreate();
        CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this);
    }
    @Override
    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
        // you will get send test display units here
    }
}
```

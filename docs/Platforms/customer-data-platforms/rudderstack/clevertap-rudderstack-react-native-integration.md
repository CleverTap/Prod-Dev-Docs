---
title: RudderStack React Native Integration
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
# Overview

The RudderStack React Native SDK allows you to track event data from your React Native applications and send it to CleverTap via RudderStack.

# Prerequisites

The following are the prerequisites for performing this integration:

* A CleverTap account
* A RudderStack account
* A Functional React Native app

# Integration

For CleverTap RudderStack React Native integration, the following are the major steps:

1. [Set Up Source and Destination at RudderStack](doc:clevertap-rudderstack-react-native-integration#set-up-source-and-destination-at-rudderstack).
2. [Install SDK in React Native Application](doc:clevertap-rudderstack-react-native-integration#install-sdk-in-react-native-application).
3. [Initialize RudderStack Client](doc:clevertap-rudderstack-react-native-integration#initialize-rudderstack-client).

## Set Up Source and Destination at RudderStack

To set up source and destination at RudderStack:

1. Navigate to *Connect* > *Sources* from the RudderStack dashboard.
2. Click **New source** to add a new source and then select *React Native* from the list of sources.

<Image title="Add New React Native Source" alt={2868} align="center" border={true} src="https://files.readme.io/98fe831-Add_New_Source.png">
  Add New React Native Source
</Image>

3. Enter the source name under *Name this source* field to identify it within your workspace and click **Continue**. The *Write Key* for this source displays, as shown in the following figure:

<Image title="Write Key for React Native" alt={2356} align="center" border={true} src="https://files.readme.io/0147ef1-Write_Key.png">
  Write Key for React Native
</Image>

4. Click **Add Destination** and select the following options from the dropdown as per your requirement:
   * *Use Existing Destination*: Select from the list of available destinations. 
   * *Create new Destination*: Create a new destination for your source.
5. Select *CleverTap* from the destination list and click **Continue**. 
6. Enter the destination name under *Name the destination* field to identify it in your workspace and click **Continue**. The *Connection Settings* page displays. 
7. Enter the following CleverTap details:
   * **Account ID**: A unique ID generated for your account. It is available under *Settings* > *Project* as the *Project ID*.
   * **Passcode**: A unique code generated for your account. It is available under *Settings* > *Project* as the *Passcode*.
   * **Token**: A unique code generated for your account. It is available under *Settings* > *Project* as *Token*.
   * **Region**: Server Only. This is the region assigned to your CleverTapaccount. Refer to the following table to identify it: 

| CleverTap Dashboard URL                                                                                | Region            |
| :----------------------------------------------------------------------------------------------------- | :---------------- |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/)   | India             |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/)   | Singapore         |
| [https://us1.dashboard.clevertap.com/login.html#/](https://us1.dashboard.clevertap.com/login.html#/)   | US                |
| [https://sk1.dashboard.clevertap.com/login.html#/](https://sk1.dashboard.clevertap.com/login.html#/)   | South Korea       |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/)   | None              |
| [https://aps3.dashboard.clevertap.com/login.html#/](https://aps3.dashboard.clevertap.com/login.html#/) | Indonesia         |
| [https://mec1.dashboard.clevertap.com/login.html#/](https://mec1.dashboard.clevertap.com/login.html#/) | Middle East (UAE) |

<Image title="Enter CleverTap Details" alt={512} align="center" width="smart" border={true} src="https://files.readme.io/342a1bb-Connection_Settings_-_1.png">
  Enter CleverTap Details
</Image>

8. Select the **Enable tracking for anonymous user** option to track anonymous users in CleverTap.
9. Select the **Use Clevertap ObjectId for Mapping** option under *React Native SDK settings* section.\
   This option enables both CleverTap `objectId` and `identity` to map events from RudderStack to CleverTap.
10. Click **Continue**. The *Transformation* settings page displays.
11. Select *No transformation needed* if you do not want to customize JavaScript code functions to implement specific use cases on your event data. 

<Image title="Select Transformations" alt={1788} align="center" width="80%" border={true} src="https://files.readme.io/235b025-No_transformation_needed.png">
  Select Transformations
</Image>

12. Click **Continue**. The connection is now established between source and destination. You can check if the connection is enabled under *Connect* > *Connections*.

<Image title="View the Connection" alt={2268} align="center" border={true} src="https://files.readme.io/e2d061f-Source_and_destination_connection_enabled.png">
  View the Connection
</Image>

## Install SDK in React Native Application

We recommend installing the React Native SDK through `npm`. To add the SDK as a dependency, perform the following steps:

1. Navigate to the root of your application and add `@rudderstack/rudder-sdk-react-native` to your application as a dependency with:

```shell
npm install @rudderstack/rudder-sdk-react-native --save
```

**OR** 

```shell
yarn add @rudderstack/rudder-sdk-react-native
```

2. Add the RudderStack-CleverTap module to your app using the following code:

```shell
npm install @rudderstack/rudder-integration-clevertap-react-native
```

**OR** 

```shell
yarn add @rudderstack/rudder-integration-clevertap-react-native
```

3. Navigate to your **Android/App** folder and add the following dependency to your `app/build.gradle` file:

```groovy
implementation 'com.clevertap.android:clevertap-android-sdk:4.4.0'
implementation 'com.google.android.gms:play-services-base:17.4.0'
implementation 'com.android.installreferrer:installreferrer:2.1'
implementation 'com.google.firebase:firebase-messaging:20.2.4'
implementation project(':clevertap-react-native')
implementation 'com.github.bumptech.glide:glide:4.11.0' //Mandatory for App Inbox
implementation 'androidx.recyclerview:recyclerview:1.1.0' //Mandatory for App Inbox
implementation 'androidx.viewpager:viewpager:1.0.0' //Mandatory for App Inbox
implementation 'com.google.android.material:material:1.3.0' //Mandatory for App Inbox
implementation 'androidx.appcompat:appcompat:1.2.0' //Mandatory for App Inbox
implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.0.0"
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.1.0' // InApp
implementation 'com.google.android.exoplayer:exoplayer:2.11.5' //Optional for Audio/Video
implementation 'com.google.android.exoplayer:exoplayer-hls:2.11.5' //Optional for Audio/Video
implementation 'com.google.android.exoplayer:exoplayer-ui:2.11.5' //Optional for Audio/Video
implementation 'com.android.support:multidex:1.0.3'
```

4. Open the `iOS folder` from your terminal and install all the pod files with the following command:

```shell
pod install
```

## Initialize RudderStack Client

To initialize the RudderStack client:

1. Import the module you added above as shown below:

```javascript
import rudderClient from '@rudderstack/rudder-sdk-react-native';
import clevertap from "@rudderstack/rudder-integration-clevertap-react-native";
const CleverTap = require('clevertap-react-native');
```

2. Initialize the CleverTap-RudderStack SDK as follows:

```javascript
const config = {
  dataPlaneUrl : "YOUR_DATA_PLANE_URL", 
  logLevel: 3,
  trackAppLifecycleEvents: true,
  recordScreenViews:true,
  withFactories: [clevertap]
};
rudderClient.setup("YOUR_WRITE_KEY", config);
```

* `trackAppLifecycleEvents`: This method allows the Rudderstack SDK to capture the Lifecycle events.
* `logLevel` → This method allows you to go through the application log when developing the application.

> 📘 Data Plane URL and Write Key
>
> You can obtain *Data Plane URL* and *Write Key* from the RudderStack dashboard under *Connect* > *Connections*.

# Push User Profiles

You can call `rudderClient.identify()` for pushing user profiles whenever you want to identify the user in your application, that is, when a user logs in or registers to your application or when you want to update any user information. After installing an app on a device, your user is assigned an "anonymous" profile. The first time when the application identifies the user on the device, the "anonymous" history on the device is associated with the newly identified user. You can call this method as follows:

```javascript
rudderClient.identify("userid", {
                                name: "Name Surname",
                                email: "email@clevertap.com",
                                phone: "+919869357572",
                                gender: "F",
                                city: "Mumbai",
                                region: "Malad",
                                country: "India",
                                "MSG-email": true,
                                "MSG-sms": true,
                                "MSG-push":true,
                              })
                          }
```

Replace `userID` with the unique identifier for your application. This identifier must match the identifier selected on the CleverTap dashboard.

# Push Events

You can call `rudderClient.track()` for pushing events whenever you want to capture any user action in your application. For example, when a user views any product, and you want to capture this event, then call this method and pass the following parameters:

* Event: Product Viewed
* Event Properties: Product viewed, Category, and Amount

> 🚧 Event Property Data Types
>
> * The value of a property can be of type Integer, Long, Double, Float, Character, String, a Boolean.
> * The Date value must always be passed as $D\_EPOCH value only.

Calling a push method may vary based on the type of event as follows:

* All events except the Order Completed event:

```javascript
rudderClient.track("Checked Out", {
  Clicked_Rush_delivery_Button: true,
  total_value: 2000,
  revenue: 2000,
});
```

* Order Completed (equivalent to the Charged event in CleverTap)

```javascript
rudderClient.track("Order Completed", {
  checkout_id: "12345",
  order_id: "1234",
  affiliation: "Apple Store",
  "Payment mode": "Credit Card",
  total: 20,
  revenue: 15.0,
  shipping: 22,
  tax: 1,
  discount: 1.5,
  coupon: "Games",
  currency: "USD",
  products: [
    {
      product_id: "123",
      sku: "G-32",
      name: "Monopoly",
      price: 14,
      quantity: 1,
      category: "Games",
      url: "https://www.website.com/product/path",
      image_url: "https://www.website.com/product/path.jpg",
    },
    {
      product_id: "345",
      sku: "F-32",
      name: "UNO",
      price: 3.45,
      quantity: 2,
      category: "Games",
    },
    {
      product_id: "125",
      sku: "S-32",
      name: "Ludo",
      price: 14,
      quantity: 7,
      category: "Games",
      brand: "Ludo King",
    },
  ],
})
```

# Integrating Push Notification in Android and iOS

The steps for integrating push notifications in Android and iOS are the same as in React Native. For more information on detailed steps, refer to [React Native Push Notification](doc:react-native-push-notification).

# Advanced Features

## Debugging

You can view the debugging logs in *Android Studio*. After you run the application, navigate to *Android Studio* > *Logcat* and search by CleverTap. The printed logs display all information associated with CleverTap. You can use this log to identify your CleverTap ID, check the events and user information that are getting pushed to CleverTap, etc.

The following is an example display of a Logcat in your Android Studio:

<Image title="RudderStack Debugging" alt={2808} align="center" border={true} src="https://files.readme.io/dadf0cf-Logcat_-_Android_Studio.png">
  RudderStack Debugging
</Image>

# Troubleshooting

* **Error Message**: If you are getting the following error: 

```shell
Note: Recompile with -Xlint:deprecation for details.
D8: Cannot fit requested classes in a single dex file (# methods: 79994 > 65536 ; # fields: 68161 > 65536)
com.android.builder.dexing.DexArchiveMergerException: Error while merging dex archives:
```

* **Reason**: Android can build files only up to 65K; if it exceeds that, you will get this error.
* **Solution**: Add the following code in your `android/app/build.gradle` file:

```groovy
defaultConfig {
        multiDexEnabled true
    }
```

> ❗️ App Launched Event for Android
>
> Currently, the App Launched event is not correctly captured. However, you can use the RudderStack Lifecycle events to capture this information.

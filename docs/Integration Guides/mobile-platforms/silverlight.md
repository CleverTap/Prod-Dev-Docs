---
title: Silverlight
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
CleverTap provides a Silverlight SDK that enables app developers to track, segment, and engage their users. 

# Install the SDK

Download the latest release of the [Silverlight 8.0](http://static.clevertap.com/sdks/CleverTapSL8.0-20160520.rar) or [Silverlight 8.1](http://static.clevertap.com/sdks/CleverTapSL8.1-20160520.rar) SDK.

In the solution explorer, right click “References” and click on “Add Reference…”.

In Reference manager, browse and select the .dll file.

# Modify WMAppManifest.xml

```xml
<Capabilities>

  <!-- This permission is required to allow the CleverTap SDK to get a unique identifier for the device the app is running on -->
  <Capability Name="ID_CAP_IDENTITY_DEVICE" />

  <!-- This permission is required to allow the app to send events to CleverTap -->
  <Capability Name="ID_CAP_NETWORKING" />

  <!-- This permission is required to allow the CleverTap SDK to receive push notifications -->
  <Capability Name="ID_CAP_PUSH_NOTIFICATION" />

  <!-- This permission is required to allow the CleverTap SDK to receive display In App Notifications -->
  <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

</Capabilities>
```

# Integrate the Library

**Option A – Using XML file** 

```xml
<?xml version="1.0" encoding="utf-8"?>
<CleverTap>

  <AccountId>Your CleverTap Account ID</AccountId>
  <AccountToken>Your CleverTap Account Token</AccountToken>

  <!-- Enable / Disable Push-Toast Notifications from WizRocket [Default-true]-->
  <RegisterForToastNotifications>true</RegisterForToastNotifications>

  <!-- Enable / Disable InApp Notifications from WizRocket [Default-true]-->
  <EnableInAppNotifications>true</EnableInAppNotifications>

  <!-- If Push Enabled, provide MPNS Channel Name [Default-WizRocket]. Channel Name not required for WNS -->
  <ChannelName>WizRocket</ChannelName>

  <!-- If Authenticated push enabled, provide Service Name, mandatory for authenticated push, leave blank for unauthenticated push. 
       Service Name not required for WNS-->
  <ServiceName></ServiceName>

  <!-- Enable / Disable personalization api from CleverTap [Default-false]-->
  <EnablePersonalization>true</EnablePersonalization>

  <!-- Enable / Disable sending Geo Location to CleverTap, enable ID_CAP_LOCATION Capability in WMAppManifest.cml [Default-true] -->
  <EnableGeoLocation>true</EnableGeoLocation>

  <!-- Enable / Disable Logging from CleverTap [Default-false]-->
  <EnableLogging>true</EnableLogging>

</CleverTap>
```

Now create a CleverTapConfig instance using the following format.

```csharp
CleverTapConfig config = CleverTapConfig.ReadFromConfigFile("path to config file");
```

**Option B – Using Code**

```csharp
CleverTapConfig config = new CleverTapConfig("Your CleverTap Account ID","Your CleverTap Account Token");

/* Enable / Disable Push-Toast Notifications from WizRocket [Default-true]*/
config.RegisterForToastNotifications = true;

/* Enable / Disable InApp Notifications from WizRocket [Default-true]*/
config.EnableInAppNotifications = true;

/* If Push Enabled, provide MPNS Channel Name [Default-WizRocket]. Channel Name not required for WNS */
config.ChannelName = "WizRocket";

/* If Authenticated Push Enabled, provide Service Name, Mandatory for Authenticated push, Do not initalize for unauthenticated push.
  Service Name not required for WNS*/
config.ServiceName = "ServiceName";

/* Enable / Disable personalization API [Default-false]*/
config.EnablePersonalization= true;

/* Enable / Disable sending Geo Location to CleverTap, enable ID_CAP_LOCATION Capability in WMAppManifest.cml [Default-true]*/
config.EnableGeoLocation = true;

/*Enable / Disable Logging from CleverTap [Default-false]*/
config.EnableLogging = false;
```

Once you have the CleverTapConfig Instance ready, initialize the following in your InitializePhoneApplication() function of App.xaml.cs.

```csharp
RootFrame = new PhoneApplicationFrame();
CleverTapConfig config = /*Initialize CleverTapConfig by one of the above methods*/
CleverTapApi CleverTapInstance = CleverTapApi.Init(config);
RootFrame.Navigated += CleverTapInstance.Navigated;

// You can save the above instance as a static variable in App.xaml.cs, for it to be
// accessed throughout the application.
// Initialized Instance can always be accessed again by:
// CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();

// Note: Improper initializations throw the following exception: CleverTapInitException
```

**Accessing CleverTap Instance** 

```csharp
CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
```

**Connect Lifecycle Events**

```csharp
// Hook up the Application_Closing function inside App.xaml.cs:
private void Application_Closing(object sender, ClosingEventArgs e)
{
 CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
 CleverTapInstance.RaiseAppClosed();
}

// Hook up the Application_Deactivated e function inside App.xaml.cs:
private void Application_Deactivated(object sender, DeactivatedEventArgs e)
{
  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
  CleverTapInstance.RaiseAppSuspended();
}

// Hook up the Application_Activated event inside App.xaml.cs:
private void Application_Activated(object sender, ActivatedEventArgs e)
{
  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
  CleverTapInstance.RaiseAppResumed();
}
```

**Enabling Push Notifications**\
Ensure “Toast Capable” is set to “Yes” in package.appxmanifest.

**Option A : Register using CleverTap**\
If you want CleverTap to register with MPNS / WNS, make sure you have enabled RegisterForToastNotifications in CleverTapConfig.

If you wish to be notified after receiving notification URI, please register your callback in CleverTapConfig.

```csharp
config.NotificationUriAvailableCallBack += <Your CallBack>;
```

**Option B : Self Registration**\
If you have already registered with MPNS / WNS, you can push your Notification Uri to CleverTap.

```csharp
// Note : For MPNS, you need to supply a channelName. You can use your mobile application name
// as channelName or else it will default to ‘WizRocket’. If you are using authenticated notifications,
// then pass serviceName which is the Subject Name / Common Name in the certificate uploaded to MPNS
CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
// For MPNS
CleverTapInstance.SendMPNSNotificationUri(MPNSUri)
// For WNS
CleverTapInstance.SendWNSNotificationUri(WNSUri)
```

If using WNS, add your WNS credentials in CleverTap Dashboard via Settings Dashboard → Push Notifications.

WNS is applicable only for Silverlight 8.1.

**Handling In-App Notifications**\
Make sure you have enabled EnableInAppNotifications in CleverTapConfig. If you want to suppress In-App on particular pages, call the following method on page load.

```csharp
// this will supress in-app notifications on the particular page where this is specified
CleverTapInstance.SuppressInAppForThisPage();
```

# Intro to User Profiles

CleverTap stores the user’s demographic data (gender, age, location), app and website interevents, campaign visits and transaction history to give you a complete picture for every user.

A User Profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the User Profile starts out as “Anonymous” – which means that the profile does not yet contain any identifiable information about the user. You can choose to enrich the profile with attributes like name, age, customer id, etc.

Here’s an example of adding a name and an age to the user’s profile.

```csharp
Dictionary<string, object> profileUpdate = new Dictionary<string, object>();
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, "Jack Montana"); //String
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Age, 28);
CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
CleverTapInstance.Profile.Push(profileUpdate);
```

CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

For complete User Profile documentation, please see the [User Profile API Endpoints](doc:user-profile-object).

# Intro to User Events

A User Event is an event that a user takes in your mobile application. CleverTap records the event on the User Profile, using an Event Name and optional associated key:value-based Event Properties. You can then segment users, target and personalize messaging based on both the Event Name and specific Event Properties.

An example of recording a User Event called Product Viewed.

```csharp
// event without properties
CleverTapInstance.Event.Push("Product viewed");
```

An example of recording a User Event called Product Viewed with Properties.

```csharp
// event with properties
Dictionary<string, object> evtProps = new Dictionary<string, object>();
evtProps.Add("Product name", "Casio Chronograph Watch");
evtProps.Add("Category", "Mens Accessories");
evtProps.Add("Price", 59.99);

CleverTapInstance.Event.Push("Product viewed", evtProps);

/**
 * Data types
 * The value of a property can be either a DateTime, an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When you pass the value of the property as DateTime, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```

Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

For a complete guide to recording User Events, please see [User Profile API Endpoints](doc:user-profile-object).

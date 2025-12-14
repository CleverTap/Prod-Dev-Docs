---
title: WinRT
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
CleverTap provides a WinRT SDK that enables app developers to track, segment, and engage their users. 

# Install the SDK

[Download](http://static.clevertap.com/sdks/CleverTapWinRT.20150926.rar) the latest release of the SDK

In the solution explorer, right click “References” and click on “Add Reference…”.

In Reference manager, browse and select CleverTapWinRTSDK.dll.

# Integrate the Library

**Add Required Permissions**\
In order for the library to work, make sure that you’re requesting the following permissions for your app inside Package.appxmanifest.xml:

* Internet Client and Server

**Initialize CleverTapApi Instance**\
 Following initialization must be done in OnLaunched event of App.xaml.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
  CleverTapApi CleverTapInstance = CleverTapApi.Init("Your CleverTap Account ID","Your CleverTap Account Token", e.Arguments);
}

// You can save the above instance as a static variable in App.xaml.cs, for it to be
// accessed throughout the application.
// Initialized Instance can always be accessed again by:
// CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();

// Note: Improper initializations throw the following exception: CleverTapInitException
```

Your App constructor should look like this.

```csharp
public App()
{
  this.InitializeComponent();
  this.Suspending += this.OnSuspending;
  this.Resuming += this.OnResuming;
}
```

**Accessing CleverTap Instance**

```csharp
CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
```

**Connecting OnResuming and OnSuspending Functions Inside App.xaml.cs** 

```csharp
// Hook up the RaiseAppSuspended in your OnSuspending function inside App.xaml.cs:
private void OnSuspending(object sender, SuspendingEventArgs e)
{
  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
  CleverTapInstance.RaiseAppSuspended();
}

// Hook up RaiseAppResumed in your OnResuming function in the App.xaml.cs:
private void OnResuming(object sender, object e)
{
  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
  CleverTapInstance.RaiseAppResumed();
}
```

**Page Events to Track Number of Pages**\
Page Events are required to be called from inside every Page’s OnNavigatedTo event.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  base.OnNavigatedTo(e);
  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();
  CleverTapInstance.Navigated();
}
```

**Tracking App Launched via Another App (optional)**\
If your application is opened from another Windows app by using the Uniform Resource Identifier (URI) scheme, you can capture the UTM parameters as described below.

```csharp
// In the OnActivated method of App.xaml.cs, add the following code
// (Tip: You can either use method #1, or method #2 as shown below):

protected override void OnActivated(IActivatedEventArgs args)
{
 base.OnActivated(args);
 if (args.Kind == ActivationKind.Protocol)
 {
 ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
 // METHOD #1:Handle URI activation
 // The received URI is eventArgs.Uri.AbsoluteUri
 // save the returning instance in a global variable for later use
 CleverTapApi.Init(CleverTapAccountId, CleverTapToken, eventArgs.Uri.AbsoluteUri);
 }

 // METHOD #2: if you already have dictionary of parameters just pass the params
 // save the returning instance in a global variable for later use
 CleverTapApi.Init(CleverTapAccountId, CleverTapToken, paramDictionary);
}
```

**Enabling Push Notifications**\
Ensure “Toast Capable” is set to “Yes” in package.appxmanifest.

Push notifications are sent via [Windows Push Notification Services](https://msdn.microsoft.com/en-us/library/windows/apps/Hh913756.aspx).

```json
// If you have already registered with WNS, you can push your Notification Uri to CleverTap:
CleverTapInstance.push.SendWNSNotificationUri(string channelUri)

// If you want CleverTap to register with WNS, you can register in OnLaunched event of App.xaml.
CleverTapInstance.Push.Register();
```

Add your WNS credentials in the Dashboard via Settings Dashboard → Push Notifications.

**Enabling In-App Notifications**\
To register a particular page for in-app notifications, in the On Page Loaded method of that page, call the CleverTapInstance.Push.InAppListener() method.

```csharp
// Add the listener only inside Page_Loaded event and not any other event.
private void ProfilePage_Loaded(object sender, RoutedEventArgs e
{
  CleverTapInstance.Push.InAppListener();
}
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
CleverTapInstance.Profile.Push(profileUpdate);
```

CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

For the complete User Profile documentation, see the [User Profile API Endpoints](doc:user-profile-object).

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

For a complete guide to recording User Events, please see the [Event API Endpoints](doc:events-object).

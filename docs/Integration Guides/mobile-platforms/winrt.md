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
**Add Required Permissions**
In order for the library to work, make sure that you’re requesting the following permissions for your app inside Package.appxmanifest.xml:
  * Internet Client and Server

**Initialize CleverTapApi Instance**
 Following initialization must be done in OnLaunched event of App.xaml.
[block:code]
{
  "codes": [
    {
      "code": "protected override void OnLaunched(LaunchActivatedEventArgs e)\n{\n  CleverTapApi CleverTapInstance = CleverTapApi.Init(\"Your CleverTap Account ID\",\"Your CleverTap Account Token\", e.Arguments);\n}\n\n// You can save the above instance as a static variable in App.xaml.cs, for it to be\n// accessed throughout the application.\n// Initialized Instance can always be accessed again by:\n// CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n\n// Note: Improper initializations throw the following exception: CleverTapInitException",
      "language": "csharp"
    }
  ]
}
[/block]
Your App constructor should look like this.
[block:code]
{
  "codes": [
    {
      "code": "public App()\n{\n  this.InitializeComponent();\n  this.Suspending += this.OnSuspending;\n  this.Resuming += this.OnResuming;\n}",
      "language": "csharp"
    }
  ]
}
[/block]
**Accessing CleverTap Instance**
[block:code]
{
  "codes": [
    {
      "code": "CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();",
      "language": "csharp"
    }
  ]
}
[/block]
**Connecting OnResuming and OnSuspending Functions Inside App.xaml.cs** 
[block:code]
{
  "codes": [
    {
      "code": "// Hook up the RaiseAppSuspended in your OnSuspending function inside App.xaml.cs:\nprivate void OnSuspending(object sender, SuspendingEventArgs e)\n{\n  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n  CleverTapInstance.RaiseAppSuspended();\n}\n\n// Hook up RaiseAppResumed in your OnResuming function in the App.xaml.cs:\nprivate void OnResuming(object sender, object e)\n{\n  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n  CleverTapInstance.RaiseAppResumed();\n}",
      "language": "csharp"
    }
  ]
}
[/block]
**Page Events to Track Number of Pages**
Page Events are required to be called from inside every Page’s OnNavigatedTo event.
[block:code]
{
  "codes": [
    {
      "code": "protected override void OnNavigatedTo(NavigationEventArgs e)\n{\n  base.OnNavigatedTo(e);\n  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n  CleverTapInstance.Navigated();\n}",
      "language": "csharp"
    }
  ]
}
[/block]
**Tracking App Launched via Another App (optional)**
If your application is opened from another Windows app by using the Uniform Resource Identifier (URI) scheme, you can capture the UTM parameters as described below.
[block:code]
{
  "codes": [
    {
      "code": "// In the OnActivated method of App.xaml.cs, add the following code\n// (Tip: You can either use method #1, or method #2 as shown below):\n\nprotected override void OnActivated(IActivatedEventArgs args)\n{\n base.OnActivated(args);\n if (args.Kind == ActivationKind.Protocol)\n {\n ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;\n // METHOD #1:Handle URI activation\n // The received URI is eventArgs.Uri.AbsoluteUri\n // save the returning instance in a global variable for later use\n CleverTapApi.Init(CleverTapAccountId, CleverTapToken, eventArgs.Uri.AbsoluteUri);\n }\n\n // METHOD #2: if you already have dictionary of parameters just pass the params\n // save the returning instance in a global variable for later use\n CleverTapApi.Init(CleverTapAccountId, CleverTapToken, paramDictionary);\n}",
      "language": "csharp"
    }
  ]
}
[/block]
**Enabling Push Notifications**
Ensure “Toast Capable” is set to “Yes” in package.appxmanifest.

Push notifications are sent via [Windows Push Notification Services](https://msdn.microsoft.com/en-us/library/windows/apps/Hh913756.aspx).
[block:code]
{
  "codes": [
    {
      "code": "// If you have already registered with WNS, you can push your Notification Uri to CleverTap:\nCleverTapInstance.push.SendWNSNotificationUri(string channelUri)\n\n// If you want CleverTap to register with WNS, you can register in OnLaunched event of App.xaml.\nCleverTapInstance.Push.Register();\n",
      "language": "json"
    }
  ]
}
[/block]
Add your WNS credentials in the Dashboard via Settings Dashboard → Push Notifications.

**Enabling In-App Notifications**
To register a particular page for in-app notifications, in the On Page Loaded method of that page, call the CleverTapInstance.Push.InAppListener() method.
[block:code]
{
  "codes": [
    {
      "code": "// Add the listener only inside Page_Loaded event and not any other event.\nprivate void ProfilePage_Loaded(object sender, RoutedEventArgs e\n{\n  CleverTapInstance.Push.InAppListener();\n}",
      "language": "csharp"
    }
  ]
}
[/block]
# Intro to User Profiles
CleverTap stores the user’s demographic data (gender, age, location), app and website interevents, campaign visits and transaction history to give you a complete picture for every user.

A User Profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the User Profile starts out as “Anonymous” – which means that the profile does not yet contain any identifiable information about the user. You can choose to enrich the profile with attributes like name, age, customer id, etc.

Here’s an example of adding a name and an age to the user’s profile.
[block:code]
{
  "codes": [
    {
      "code": "Dictionary<string, object> profileUpdate = new Dictionary<string, object>();\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, \"Jack Montana\"); //String\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Age, 28);\nCleverTapInstance.Profile.Push(profileUpdate);",
      "language": "csharp"
    }
  ]
}
[/block]
CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

For the complete User Profile documentation, see the [User Profile API Endpoints](doc:user-profile-object).

# Intro to User Events
A User Event is an event that a user takes in your mobile application. CleverTap records the event on the User Profile, using an Event Name and optional associated key:value-based Event Properties. You can then segment users, target and personalize messaging based on both the Event Name and specific Event Properties.

An example of recording a User Event called Product Viewed.
[block:code]
{
  "codes": [
    {
      "code": "// event without properties\nCleverTapInstance.Event.Push(\"Product viewed\");",
      "language": "csharp"
    }
  ]
}
[/block]
An example of recording a User Event called Product Viewed with Properties.
[block:code]
{
  "codes": [
    {
      "code": "// event with properties\nDictionary<string, object> evtProps = new Dictionary<string, object>();\nevtProps.Add(\"Product name\", \"Casio Chronograph Watch\");\nevtProps.Add(\"Category\", \"Mens Accessories\");\nevtProps.Add(\"Price\", 59.99);\n\nCleverTapInstance.Event.Push(\"Product viewed\", evtProps);\n\n/**\n * Data types\n * The value of a property can be either a DateTime, an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When you pass the value of the property as DateTime, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "csharp"
    }
  ]
}
[/block]
Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

For a complete guide to recording User Events, please see the [Event API Endpoints](doc:events-object).
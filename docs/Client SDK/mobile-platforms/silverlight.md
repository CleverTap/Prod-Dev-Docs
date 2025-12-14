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
[block:code]
{
  "codes": [
    {
      "code": "<Capabilities>\n\n  <!-- This permission is required to allow the CleverTap SDK to get a unique identifier for the device the app is running on -->\n  <Capability Name=\"ID_CAP_IDENTITY_DEVICE\" />\n\n  <!-- This permission is required to allow the app to send events to CleverTap -->\n  <Capability Name=\"ID_CAP_NETWORKING\" />\n\n  <!-- This permission is required to allow the CleverTap SDK to receive push notifications -->\n  <Capability Name=\"ID_CAP_PUSH_NOTIFICATION\" />\n\n  <!-- This permission is required to allow the CleverTap SDK to receive display In App Notifications -->\n  <Capability Name=\"ID_CAP_WEBBROWSERCOMPONENT\" />\n\n</Capabilities>",
      "language": "xml"
    }
  ]
}
[/block]
# Integrate the Library

**Option A – Using XML file** 
[block:code]
{
  "codes": [
    {
      "code": "<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<CleverTap>\n\n  <AccountId>Your CleverTap Account ID</AccountId>\n  <AccountToken>Your CleverTap Account Token</AccountToken>\n\n  <!-- Enable / Disable Push-Toast Notifications from WizRocket [Default-true]-->\n  <RegisterForToastNotifications>true</RegisterForToastNotifications>\n\n  <!-- Enable / Disable InApp Notifications from WizRocket [Default-true]-->\n  <EnableInAppNotifications>true</EnableInAppNotifications>\n\n  <!-- If Push Enabled, provide MPNS Channel Name [Default-WizRocket]. Channel Name not required for WNS -->\n  <ChannelName>WizRocket</ChannelName>\n\n  <!-- If Authenticated push enabled, provide Service Name, mandatory for authenticated push, leave blank for unauthenticated push. \n       Service Name not required for WNS-->\n  <ServiceName></ServiceName>\n\n  <!-- Enable / Disable personalization api from CleverTap [Default-false]-->\n  <EnablePersonalization>true</EnablePersonalization>\n\n  <!-- Enable / Disable sending Geo Location to CleverTap, enable ID_CAP_LOCATION Capability in WMAppManifest.cml [Default-true] -->\n  <EnableGeoLocation>true</EnableGeoLocation>\n\n  <!-- Enable / Disable Logging from CleverTap [Default-false]-->\n  <EnableLogging>true</EnableLogging>\n\n</CleverTap>",
      "language": "xml"
    }
  ]
}
[/block]
Now create a CleverTapConfig instance using the following format.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapConfig config = CleverTapConfig.ReadFromConfigFile(\"path to config file\");",
      "language": "csharp"
    }
  ]
}
[/block]
**Option B – Using Code**
[block:code]
{
  "codes": [
    {
      "code": "CleverTapConfig config = new CleverTapConfig(\"Your CleverTap Account ID\",\"Your CleverTap Account Token\");\n\n/* Enable / Disable Push-Toast Notifications from WizRocket [Default-true]*/\nconfig.RegisterForToastNotifications = true;\n\n/* Enable / Disable InApp Notifications from WizRocket [Default-true]*/\nconfig.EnableInAppNotifications = true;\n\n/* If Push Enabled, provide MPNS Channel Name [Default-WizRocket]. Channel Name not required for WNS */\nconfig.ChannelName = \"WizRocket\";\n\n/* If Authenticated Push Enabled, provide Service Name, Mandatory for Authenticated push, Do not initalize for unauthenticated push.\n  Service Name not required for WNS*/\nconfig.ServiceName = \"ServiceName\";\n\n/* Enable / Disable personalization API [Default-false]*/\nconfig.EnablePersonalization= true;\n\n/* Enable / Disable sending Geo Location to CleverTap, enable ID_CAP_LOCATION Capability in WMAppManifest.cml [Default-true]*/\nconfig.EnableGeoLocation = true;\n\n/*Enable / Disable Logging from CleverTap [Default-false]*/\nconfig.EnableLogging = false;\n",
      "language": "csharp"
    }
  ]
}
[/block]
Once you have the CleverTapConfig Instance ready, initialize the following in your InitializePhoneApplication() function of App.xaml.cs.
[block:code]
{
  "codes": [
    {
      "code": "RootFrame = new PhoneApplicationFrame();\nCleverTapConfig config = /*Initialize CleverTapConfig by one of the above methods*/\nCleverTapApi CleverTapInstance = CleverTapApi.Init(config);\nRootFrame.Navigated += CleverTapInstance.Navigated;\n\n// You can save the above instance as a static variable in App.xaml.cs, for it to be\n// accessed throughout the application.\n// Initialized Instance can always be accessed again by:\n// CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n\n// Note: Improper initializations throw the following exception: CleverTapInitException",
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
**Connect Lifecycle Events**
[block:code]
{
  "codes": [
    {
      "code": "// Hook up the Application_Closing function inside App.xaml.cs:\nprivate void Application_Closing(object sender, ClosingEventArgs e)\n{\n CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n CleverTapInstance.RaiseAppClosed();\n}\n\n// Hook up the Application_Deactivated e function inside App.xaml.cs:\nprivate void Application_Deactivated(object sender, DeactivatedEventArgs e)\n{\n  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n  CleverTapInstance.RaiseAppSuspended();\n}\n\n// Hook up the Application_Activated event inside App.xaml.cs:\nprivate void Application_Activated(object sender, ActivatedEventArgs e)\n{\n  CleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n  CleverTapInstance.RaiseAppResumed();\n}",
      "language": "csharp"
    }
  ]
}
[/block]
**Enabling Push Notifications**
Ensure “Toast Capable” is set to “Yes” in package.appxmanifest.

**Option A : Register using CleverTap**
If you want CleverTap to register with MPNS / WNS, make sure you have enabled RegisterForToastNotifications in CleverTapConfig.

If you wish to be notified after receiving notification URI, please register your callback in CleverTapConfig.
[block:code]
{
  "codes": [
    {
      "code": "config.NotificationUriAvailableCallBack += <Your CallBack>;",
      "language": "csharp"
    }
  ]
}
[/block]
**Option B : Self Registration**
If you have already registered with MPNS / WNS, you can push your Notification Uri to CleverTap.
[block:code]
{
  "codes": [
    {
      "code": "// Note : For MPNS, you need to supply a channelName. You can use your mobile application name\n// as channelName or else it will default to ‘WizRocket’. If you are using authenticated notifications,\n// then pass serviceName which is the Subject Name / Common Name in the certificate uploaded to MPNS\nCleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\n// For MPNS\nCleverTapInstance.SendMPNSNotificationUri(MPNSUri)\n// For WNS\nCleverTapInstance.SendWNSNotificationUri(WNSUri)",
      "language": "csharp"
    }
  ]
}
[/block]
If using WNS, add your WNS credentials in CleverTap Dashboard via Settings Dashboard → Push Notifications.

WNS is applicable only for Silverlight 8.1.

**Handling In-App Notifications**
Make sure you have enabled EnableInAppNotifications in CleverTapConfig. If you want to suppress In-App on particular pages, call the following method on page load.
[block:code]
{
  "codes": [
    {
      "code": "// this will supress in-app notifications on the particular page where this is specified\nCleverTapInstance.SuppressInAppForThisPage();",
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
      "code": "Dictionary<string, object> profileUpdate = new Dictionary<string, object>();\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, \"Jack Montana\"); //String\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Age, 28);\nCleverTapApi CleverTapInstance = CleverTapApi.GetInstance();\nCleverTapInstance.Profile.Push(profileUpdate);",
      "language": "csharp"
    }
  ]
}
[/block]
CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

For complete User Profile documentation, please see the [User Profile API Endpoints](doc:user-profile-object).

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

For a complete guide to recording User Events, please see [User Profile API Endpoints](doc:user-profile-object).
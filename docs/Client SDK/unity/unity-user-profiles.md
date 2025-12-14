---
title: Unity User Profiles
excerpt: Learn how to manage user profiles with Unity and Unity Native SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# User Profiles

User profiles help you personalize engagement by associating each app user with identity, preferences, contact details, and behavioral traits. Using CleverTap’s Unity SDK, you can create and update user profiles, set multi-value attributes, manage identifiers, and modify profile properties in real time.

This guide explains how to work with user profile data to power personalized campaigns, analytics, and segmentation.

## Create a User Profile (OnUserLogin)

Use `OnUserLogin` to create a new user profile or re-identify an existing user after login or signup. This method is essential for associating events with a unique identity across sessions and devices.

```csharp
Dictionary<string, string> newProps = new Dictionary<string, string>();
newProps.Add("Name", "Jack Montana");        // User's full name
newProps.Add("Identity", "61026032");        // Unique identity string (must be consistent across devices)
newProps.Add("Email", "jack@gmail.com");     // User's email address
newProps.Add("Phone", "+14155551234");       // Phone number with country code
newProps.Add("Gender", "M");                 // Accepted values: "M" or "F"
CleverTap.OnUserLogin(newProps);
// Note: For SDK v2.4.2 or below, use CleverTapBinding instead.
```

## Update User Profile (PushProfile)

Use `ProfilePush` to update user attributes such as name, email, or phone number.

```csharp
Dictionary<string, string> newProps = new Dictionary<string, string>();
newProps.Add("Name", "Jack Montana");    // String
newProps.Add("Email", "jack@gmail.com"); // User’s email address
newProps.Add("Phone", "+14155551234");   // Phone number with country code starting with +
newProps.Add("Gender", "M");             // Can be either M or F
CleverTap.ProfilePush(newProps);
// Note: For SDK v2.4.2 or below, use CleverTapBinding instead.
```

## Get CleverTap Reference ID

Retrieve the unique CleverTap ID assigned to the current user. This is useful for debugging, server-side linking, or cross-device identification.

```csharp
// For synchronous use (works on both Android and iOS)
string CleverTapID = CleverTap.ProfileGetCleverTapID();
```

```csharp
// For Android: Use async callback method to get the CleverTap ID after initialization
#if UNITY_ANDROID
CleverTap.OnCleverTapInitCleverTapIdCallback += ctId => {
    Debug.Log("CleverTap ID: " + ctId);
};
CleverTap.GetCleverTapID();
#endif
```

> 🚧 Platform-Specific Behavior
>
> iOS returns the ID immediately. Android requires the async callback above after SDK initialization.

## Set Multi Values For Key

Assign multiple values to a single profile key using `ProfileSetMultiValuesForKey`.

```csharp
List<string> StringList = new List<string>();
StringList.Add("one");
StringList.Add("two");

CleverTap.ProfileSetMultiValuesForKey("userProps", StringList);
```

## Add Multi Value For Key

Add new values to an existing multi-value key.

```csharp
List<string> StringList1 = new List<string>();
StringList1.Add("three");
StringList1.Add("four");

CleverTap.ProfileAddMultiValuesForKey("multiIOS", StringList1);
```

## Remove Multi Value For Key

Remove one or more values from an existing multi-value key.

```csharp
List<string> StringList2 = new List<string>();
StringList2.Add("two");

CleverTap.ProfileRemoveMultiValuesForKey("userProps", StringList2);
```

## Increment a User Profile property

Increase numeric values for user properties, such as rewards or scores.

```csharp
CleverTap.ProfileIncrementValueForKey("add_int",2);
CleverTap.ProfileIncrementValueForKey("add_double",3.5);
```

## Decrement a User Profile property

Decrease values for numeric user profile properties.

```csharp
CleverTap.ProfileDecrementValueForKey("minus_int",2);
CleverTap.ProfileDecrementValueForKey("minus_double",3.5);
```

## Set Location to User Profile

Associate the user's last known location with their profile.

```csharp
CleverTap.SetLocation(34.147785, -118.144516); // Latitude, Longitude
// Useful for geo-targeted campaigns, regional analytics, and personalized content.
```

For a complete implementation guide on managing user profiles, refer to [User Profiles](doc:concepts-user-profiles).

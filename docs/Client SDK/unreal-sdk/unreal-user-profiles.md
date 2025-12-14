---
title: Unreal User Profiles
excerpt: Learn how to update user profiles with CleverTap Unreal SDK
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

User profiles help you associate key attributes such as name, email, preferences, and behavior to personalize messaging and track engagement over time.

## On User Login

Use `OnUserLogin()` when a user is identified and logs into your app. This method:

* Sets or updates the user's identity
* Merges anonymous session data into the identified user profile
* Allows you to set properties such as name, email, and preferences

```cplusplus
FCleverTapProperties Profile;
Profile.Add(TEXT("Name"), TEXT("Jack Montana"));
Profile.Add(TEXT("Identity"), 61026032);               // String or Number
Profile.Add(TEXT("Email"), TEXT("jack@gmail.com"));    // Email
Profile.Add(TEXT("Phone"), TEXT("+14155551234"));      // With country code
Profile.Add(TEXT("Gender"), TEXT("M"));                // M or F
Profile.Add(TEXT("DOB"), FCleverTapDate(1953, 3, 13)); // Date object

// Notification preferences
Profile.Add(TEXT("MSG-email"), false);
Profile.Add(TEXT("MSG-push"), true);
Profile.Add(TEXT("MSG-sms"), false);
Profile.Add(TEXT("MSG-whatsapp"), true);

// Multi-value properties
TArray<FString> Stuff;
Stuff.Add(TEXT("bag"));
Stuff.Add(TEXT("shoes"));
Profile.Add(TEXT("MyStuff"), Stuff);

ICleverTapInstance& CleverTap = GEngine->GetEngineSubsystem<UCleverTapSubsystem>()->SharedInstance();
CleverTap.OnUserLogin(Profile);
```

> 📘 Identify a User with `OnUserLogin()`
>
> Call `OnUserLogin()` only once per session to identify a new user. The SDK links the anonymous profile to a known identity and resets the tracking context. If you call it again in the same session, the SDK will ignore it unless the user identity has changed.

## Update a User Profile

To update or enrich a user profile after login, use `PushProfile()`. You can update any single or multi-value property previously set, or add new ones.

```cplusplus
FCleverTapProperties Profile;
Profile.Add(TEXT("MSG-push"), false);                    // Update
Profile.Add(TEXT("Tz"), TEXT("Asia/Kolkata"));           // Add new
Profile.Add(TEXT("Plan Type"), TEXT("Silver"));
Profile.Add(TEXT("Score"), 100);

ICleverTapInstance& CleverTap = GEngine->GetEngineSubsystem<UCleverTapSubsystem>()->SharedInstance();
CleverTap.PushProfile(Profile);
```

Use `PushProfile()` at any point to update a user’s profile without changing their identity.

For a list of supported user profile keys, refer to [Updating the User Profile](https://developer.clevertap.com/docs/concepts-user-profiles#updating-the-user-profile).

## Increment or Decrement Properties

You can increment or decrement numeric profile properties such as points, scores, or usage counters. These operations support `int32`, `int64`, `float`, and `double`.

```cplusplus
ICleverTapInstance& CleverTap = GEngine->GetEngineSubsystem<UCleverTapSubsystem>()->SharedInstance();

CleverTap.IncrementValue(TEXT("Score"), 50);
CleverTap.DecrementValue(TEXT("Score"), 25);

CleverTap.IncrementValue(TEXT("Score"), 5.5);
CleverTap.DecrementValue(TEXT("Score"), 3.14);
```

---
title: Flutter Product Experiences
excerpt: Learn how to set up Remote Config for your Flutter application.
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

With the CleverTap Flutter SDK, you can create variables on your app that take on new values from the CleverTap dashboard. Using variables in the CleverTap dashboard allows you to roll out changes without pushing an update through the Apple App Store/Google Play Store. These can also be used in A/B tests to test features for only a percentage of your users.

You can define variables using the CleverTap Flutter SDK. When you define a variable in your code, you can sync it to the CleverTap Dashboard via the provided SDK methods.

<Callout icon="📘" theme="info">
  ### SDK Prerequisite

  Before you proceed, check that you are using Flutter SDK v1.7.0 or higher to use the Flutter Product Experiences feature.
</Callout>

# Supported Variable Types

Currently, CleverTap SDK supports the following variable types:

- String
- boolean
- Map Object
- Number
- int
- float
- double

Web platform only supports primitive data types: String, Number, and Boolean

# Define Variables

Variables can be defined using the `defineVariables` method. You must provide the names and default values of the variables using a Map Object.

```javascript Dart
const variables = {
              'flutter_var_string': 'flutter_var_string_value',
              'flutter_var_map': {
                'flutter_var_map_string': 'flutter_var_map_value'
              },
              'flutter_var_int': 6,
              'flutter_var_float': 6.9,
              'flutter_var_boolean': true
            };
CleverTapPlugin.defineVariables(variables);
```

# Define File Type Variables

CleverTap supports file type variables for CleverTap Flutter SDK v3.0.0 and above. Supported file types include but are not limited to images (jpg, jpeg, png, gif), text files, and PDF files.

You can define a new file type variable using the following code:

```coffeescript JavaScript
CleverTapPlugin.defineFileVariable("fileVariable");
```

# Setup Callbacks

CleverTap Flutter SDK provides several callbacks for the developer to receive feedback from the SDK. You can use them per your requirement, using all of them is not mandatory.

## Flutter SDK version lower than 3.0.0

The CleverTap Flutter SDK 3.0.0 and lower supports the following callbacks:

- Status of fetch variables request
- `onVariablesChanged`
- `onValueChanged`

## Status of Variables Fetch Request

This method provides a boolean flag to ensure that the variables are successfully fetched from the server.

```javascript Dart
CleverTapPlugin.setState(() async {
    bool? success = await CleverTapPlugin.fetchVariables();
    print("fetchVariables result: " + success.toString());
});
```

## `onVariablesChanged`

This callback is invoked when variables are initialized with values fetched from the server. It is called each time new values are fetched.

```javascript Dart
CleverTapPlugin.onVariablesChanged((variables) {
      print("onVariablesChanged: " + variables.toString());
});
```

## `onValueChanged`

This callback is invoked when the value of the variable changes. You must provide the name of the variable whose value needs to be observed.

```javascript Dart
CleverTapPlugin.onValueChanged('flutter_var_string', (variable) {
      print("onValueChanged: " + variable.toString());
});
```

## Callbacks supported for Flutter SDK version 3.0.0 and above

The following callbacks are supported for the Flutter SDK version 3.3.0 and above:

- `onOneTimeVariablesChanged`
- `onVariablesChangedAndNoDownloadsPending`
- `onceVariablesChangedAndNoDownloadsPending`
- `onFileValueChanged`

### `onOneTimeVariablesChanged`

This callback is invoked only once when variables are initialized or updated with server values. It is triggered on app start or when the callback is added if the values are already available.

```javascript
CleverTapPlugin.onOneTimeVariablesChanged((variables) {
    print("onOneTimeVariablesChanged: " + variables.toString());
});
```

### `onVariablesChangedAndNoDownloadsPending`

This callback is invoked every time variable values are updated and all associated files are downloaded and ready for use.

```javascript
CleverTapPlugin.onVariablesChangedAndNoDownloadsPending((variables) => {
    print('onVariablesChangedAndNoDownloadsPending', variables);
});
```

### `onceVariablesChangedAndNoDownloadsPending`

This callback is invoked only once when variable values are updated and their associated files are downloaded and ready for use. It is triggered only the first time new values are fetched and downloaded.

```javascript
CleverTapPlugin.onceVariablesChangedAndNoDownloadsPending((variables) => {
    print('onceVariablesChangedAndNoDownloadsPending', variables);
});
```

### `onFileValueChanged`

This callback is registered per file variable. It is called when the file associated with the file variable is downloaded and ready to be used.

```javascript
CleverTapPlugin.onFileValueChanged('fileVariable', (variable) => {
    print('onFileValueChanged: ', variable);
});
```

# Sync Defined Variables

After defining your variables in the code, you must send/sync variables to the server. From the CleverTap dashboard, mark a required CleverTap user profile as a test profile. For more information, refer to [Mark a User Profile as a Test Profile](https://docs.clevertap.com/docs/user-profiles#mark-a-user-profile-as-a-test-profile).

After marking the profile as a test profile, you must sync the app variables in DEBUG mode:

```javascript Dart
// 1. Define CleverTap variables 
// …
// 2. Add variables/values changed callbacks
// …
// 3. Sync CleverTap Variables from DEBUG mode/builds
CleverTapPlugin.syncVariables();
```

<Callout icon="📘" theme="info">
  ### Key Points to Remember

  - In a scenario where there is already a draft created by another user profile in the dashboard, the sync call will fail to avoid overriding important changes made by someone else. In this case, Publish or Dismiss the existing draft before you proceed with syncing variables again. However, you can override a draft you created via the sync method previously to optimize the integration experience.
  - You can receive the following Logcat logs from the CleverTap SDK:
    - Variables synced successfully.
    - Unauthorized access from a non-test profile. To address this, mark the profile as a test profile from the CleverTap dashboard.
</Callout>

# Fetch Variables During a Session

You can fetch the updated values for your CleverTap variables from the server during a session. If variables have changed, the appropriate callbacks will be fired. The provided callback provides a boolean flag that indicates if the fetch call was successful. The callback is fired regardless of whether the variables have changed or not.

```javascript Dart
CleverTapPlugin.fetchVariables().then((success) => {
      print("fetchVariables result: "+ success.toString())
});
```

# Use Fetched Variables Values

This process involves the following two major steps:

1. [Fetch variable values](doc:flutter-remote-config#fetch-variable-values).
2. [Access variable values](doc:flutter-remote-config#access-variable-values).

## Fetch Variable Values

Variables are updated automatically when server values are received. If you want to receive feedback when a specific variable is updated, use the individual callback:

```javascript Dart
CleverTapPlugin.onValueChanged('flutter_var_string', (variable) {
      print("onValueChanged: " + variable.toString());
});
```

### Re-evaluate Segments After Property Update

When implementing Product Experience segments that depend on user property changes, you must update the user properties first and then immediately fetch variables to ensure the segment evaluation reflects the latest user data.

If a user property (for example, Customer Type) is used in Product Experience segments, you must re-fetch variables after updating that property. This ensures the user sees the correct app experience.

Follow this sequence to ensure that user property changes trigger Product Experience re-evaluation:

1. Update user properties using any of the [supported method](doc:user-profiles-flutter#updating-user-profileprofile-set-method).
2. Call `fetchVariables()` to trigger re-evaluation of segments using the updated data.

#### Example Re-evaluation

```javascript Dart
// Update user property
var profile = {
    'Customer Type': 'Premium'
};
CleverTapPlugin.profileSet(profile);

// Immediately fetch variables to re-evaluate segments
CleverTapPlugin.fetchVariables().then((success) => {
    if (success == true) {
        // Product Experiences will now use the updated "Customer Type" property
        // Access your variables here or rely on existing variable callbacks
    }
});
```

<Callout icon="📘" theme="info">
  ### Note

  - Always call `fetchVariables()` immediately after any user property updates used in Product Experience segments.
  - The fetch call is asynchronous; use the callback to confirm success.
  - Existing `VariableCallback` instances are automatically triggered when variables change.
</Callout>

## Access Variable Values

Remote Variables are key-value pairs defined in the CleverTap dashboard that you can update without releasing a new app version. You can access these fetched values in the following two ways:

### Getting all variables

Fetch the complete list of Remote Variables currently available in the app.

```javascript Dart
CleverTapPlugin.getVariables().then((variables) => {
      print("getVariables: " + variables.toString())
});
```

### Getting a specific variable

Fetch the value of a particular Remote Variable by providing its key.

```javascript Dart
CleverTapPlugin.getVariable('flutter_var_string').then((variable) => {
      print('variable value for key \'flutter_var_string\': ' + variable.toString())
});
```

### Fetch A/B Experiment Variants

From CleverTap Flutter SDK v3.7.0 onwards, you can retrieve the active A/B experiment variants for the current user. This is useful when you need to access variant data for custom implementations or analytics, independent of Remote Config variables.

<Callout icon="📘" theme="info">
  ### SDK Prerequisite

  Before you proceed, check that you are using Flutter SDK v3.7.0 or higher to use this method.
</Callout>

The getVariants() method returns a list of the user's active variants. If no experiment variants are active for the current user, getVariants() returns an empty list.

Sample response:

```json
"abVariantInfo": [
        {
            "abTestId": 224197,
            "abTestName": "first test",
            "name": "Control"
        },
        {
            "abTestId": 623794,
            "abTestName": "second test",
            "name": "Control"
        },
        {
            "abTestId": 962480,
            "abTestName": "Android users",
            "name": "A"
        },
        {
            "abTestId": 453537,
            "abTestName": "String Vars2",
            "name": "A"
        }
    ]
```

<br />

<br />

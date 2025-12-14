---
title: React Native Product Experiences
excerpt: Learn how to set up Remote Config for your React Native application.
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

With the CleverTap React Native SDK, you can create variables on your app that take on new values from the CleverTap dashboard. Using variables in the CleverTap dashboard allows you to roll out changes without pushing an update through the Apple App Store/Google Play Store. These can also be used in A/B tests to test features for only a percentage of your users.

You can define variables using the CleverTap React Native SDK. When you define a variable in your code, you can sync it to the CleverTap Dashboard via the provided SDK methods.

> 📘 SDK Prerequisite
>
> Before you proceed, ensure that you are using React Native v1.1.0 or higher to use the Remote Config feature.

# Supported Variable Types

CleverTap React Native SDK supports the following variable types:

* String
* Boolean
* Dictionary
* Int
* Float
* Double
* Short
* Long
* Number
* File (supported for CleverTap SDK **v3.1.0 or higher**)

# Define Variables

Variables can be defined using the `defineVariables` method. You must provide the names and default values of the variables using a JSON Object. 

```javascript
let variables = {
              'reactnative_var_string': 'reactnative_var_string_value',
              'reactnative_var_map': {
                'reactnative_var_map_string': 'reactnative_var_map_value'
              },
              'reactnative_var_int': 6,
              'reactnative_var_float': 6.9,
              'reactnative_var_boolean': true
            };
CleverTap.defineVariables(variables);
```

## Define File Type Variables

CleverTap supports file type variables starting from CleverTap React Native SDK v3.1.0 and above. Supported file types include but are not limited to images (jpg, jpeg, png, gif), text files, and PDFs.

You can define a new file type variable using the following code:

```javascript
CleverTap.defineFileVariable('fileVariable');
```

# Sync Defined Variables

After defining your variables in the code, you must send/sync variables to the server. From the CleverTap dashboard, mark a required CleverTap user profile as a test profile. For more information, refer to [Mark a User Profile as a Test Profile](https://docs.clevertap.com/docs/user-profiles#mark-a-user-profile-as-a-test-profile).

After marking the profile as a test profile, you must sync the app variables in DEBUG mode:

```javascript
// 1. Define CleverTap variables 
// …
// 2. Add variables/values changed callbacks
// …
// 3. Sync CleverTap Variables from DEBUG mode/builds
CleverTap.syncVariables();
```

> 📘 Key Points to Remember
>
> * In a scenario where there is already a draft created by another user profile in the dashboard, the sync call will fail to avoid overriding important changes made by someone else. In this case, Publish or Dismiss the existing draft before you proceed with syncing variables again. However, you can override a draft you created via the sync method previously to optimize the integration experience.
> * You can receive the following Logcat logs from the CleverTap SDK:
>   * Variables synced successfully.
>   * Unauthorized access from a non-test profile. To address this, mark the profile as a test profile from the CleverTap dashboard.

# Fetch Variables During a Session

You can fetch the updated values for your CleverTap variables from the server during a session. If variables have changed, the appropriate callbacks will be fired. The provided callback provides a boolean flag that indicates if the fetch call was successful. The callback is fired regardless of whether the variables have changed or not.

```javascript
 CleverTap.fetchVariables((err, success) => {
    console.log('fetchVariables result: ', success);
});
```

# Use Fetched Variables Values

This process involves the following major steps:

1. [Fetch Variable Values](doc:react-native-remote-config#fetch-variable-values)
2. [Verify Status of Variables Fetch Request](doc:react-native-remote-config#verify-status-of-variables-fetch-request) 
3. [Access Variable Values](doc:react-native-remote-config#access-variable-values)

## Fetch Variable Values

Variables are updated automatically when server values are received. If you want to receive feedback when a specific variable is updated, use the individual callback:

```javascript
CleverTap.onValueChanged('reactnative_var_string', (variable) => {
    console.log('onValueChanged: ', variable);
});
```

## Verify Status of Variables Fetch Request

The `fetchVariables` method verifies if the variables were successfully retrieved from the server.

```javascript
CleverTap.fetchVariables((err, success) => {
    console.log('fetchVariables result: ', success);
});
```

### Re-evaluate Segments After Property Update

When implementing Product Experience segments that depend on user property changes, you must update the user properties first and then immediately fetch variables to ensure the segment evaluation reflects the latest user data.

If a user property (for example, Customer Type) is used in Product Experience segments, you must re-fetch variables after updating that property. This ensures the user sees the correct app experience.

Follow this sequence to ensure that user property changes trigger Product Experience re-evaluation:

1. Update user properties using any of the [supported method](doc:user-profiles-react-native#update-user-profilepush-profile). 
2. Call `fetchVariables()` to trigger re-evaluation of segments using the updated data.

#### Example Re-evaluation

```javascript
// Update user property
var profile = {
    'Customer Type': 'Premium'
};
CleverTap.profileSet(profile);

// Immediately fetch variables to re-evaluate segments
CleverTap.fetchVariables((err, success) => {
    if (success) {
        // Product Experiences will now use the updated "Customer Type" property
        // Access your variables here or rely on existing variable callbacks
    }
});
```

> 📘 Note
>
> * Always call `fetchVariables()` immediately after any user property updates used in Product Experience segments.
> * The fetch call is asynchronous; use the callback to confirm success.
> * Existing `VariableCallback` instances are automatically triggered when variables change.

## Access Variable Values

Remote Variables are key-value pairs defined in the CleverTap dashboard that you can update without releasing a new app version. You can access these fetched values in the following two ways:

### Getting all variables

Fetch the complete list of Remote Variables currently available in the app.

```javascript
CleverTap.getVariables((err, variables) => {
    console.log('getVariables: ', variables, err);
});
```

### Getting a specific variable

Fetch the value of a particular Remote Variable by providing its key.

```javascript
CleverTap.getVariable('reactnative_var_string', (err, variable) => {
    console.log(`variable value for key \'reactnative_var_string\': ${variable}`);
});
```

# Set Up Callbacks

CleverTap React Native SDK provides feedback through multiple callbacks. You can select only the required callbacks. They are as follows:

* [All Variables Callbacks](doc:react-native-remote-config#all-variables-callbacks)
* [Individual Variable Callback](doc:react-native-remote-config#individual-variable-callback) 
* [All File Variables Callback](doc:react-native-remote-config#all-file-variables-callback) 
* [File Variables Individual Callback](doc:react-native-remote-config#file-variables-individual-callback) 

## All Variables Callbacks

### `onVariablesChanged`

This callback is invoked after the variables are initialized with values fetched from the server. It is called each time new values are fetched.

```javascript
CleverTap.onVariablesChanged((variables) => {
    console.log('onVariablesChanged: ', variables);
});
```

### `onceVariablesChanged`

This callback is invoked after the variables are initialized with values fetched from the server. It is called only once.

```javascript
CleverTap.onceVariablesChanged((variables) => {
    console.log('onceVariablesChanged: ', variables);
});
```

## Individual Variable Callback

### `onValueChanged`

This callback is invoked after the variable's value changes. To monitor its value, you must provide the variable's name.

```javascript
CleverTap.onValueChanged('reactnative_var_string', (variable) => {
    console.log('onValueChanged: ', variable);
});
```

## All File Variables Callback

### `onVariablesChangedAndNoDownloadsPending`

This callback is called when no files need to be downloaded or all downloads have been completed. It is called each time new values are fetched and downloads are completed.

```javascript
CleverTap.onVariablesChangedAndNoDownloadsPending('fileVariable', (fileVariable) => {
    console.log('onVariablesChangedAndNoDownloadsPending: ', fileVariable);
});
```

### `onceVariablesChangedAndNoDownloadsPending`

This callback is called only once when no files need to be downloaded, or all downloads have been completed.

```javascript
CleverTap.onceVariablesChangedAndNoDownloadsPending('fileVariable', (fileVariable) => {
    console.log('onceVariablesChangedAndNoDownloadsPending: ', fileVariable);
});
```

## File Variables Individual Callback

### `onFileValueChanged`

This callback is called when the file variable's value is downloaded and ready. It is available only for File variables.

```javascript
CleverTap.onFileValueChanged('fileVariable', (fileVariable) => {
    console.log('onFileValueChanged: ', fileVariable);
});
```

# Troubleshooting

For troubleshooting, refer to the [iOS](https://developer.clevertap.com/docs/ios-remote-config#troubleshooting) and [Android](doc:android-remote-config#troubleshooting) product experiences documentation.

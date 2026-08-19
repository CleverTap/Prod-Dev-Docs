---
title: iOS Product Experiences
excerpt: Learn how to set up Remote Config for your iOS application.
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

With the CleverTap SDK, you can create variables on your client that take on new values from the server. Using variables in the CleverTap dashboard allows you to roll out changes without having to push an update through the App Store or Google Play. These can also be used in A/B tests to test features for only a percentage of your users.

You can define variables in the platform-specific call in our SDK. When you define a variable in your code, you can sync it to the Dashboard via the provided SDK method.

<Callout icon="📘" theme="info">
  ### SDK Prerequisite

  Before you proceed, check that you are using iOS SDK v5.0.0 or higher to use the Remote Config feature.
</Callout>

# Supported Variable Types

Currently, CleverTap SDK supports the following variable types:

- String
- Boolean
- Dictionary
- Int
- Float
- Double
- Short
- Long
- Number
- File (supported for CleverTap SDK **v7.0.0 and above**)

# Define Variables

Variables can be defined using a shared or custom CleverTap instance. The Variable is defined using the `defineVar` method, which returns an instance of a `CTVar` variable. You must provide the name and default value of the variable.

```swift Swift
// Primitive types
let var_string = CleverTap.sharedInstance()?.defineVar(name: "var_string", string: "hello, world")
let var_int = CleverTap.sharedInstance()?.defineVar(name: "var_int", integer: 10)
let var_bool = CleverTap.sharedInstance()?.defineVar(name: "var_bool", boolean: true)
let var_float = CleverTap.sharedInstance()?.defineVar(name: "var_float", float: 6.0)
let var_double = CleverTap.sharedInstance()?.defineVar(name: "var_double", double: 60.999)
let var_short = CleverTap.sharedInstance()?.defineVar(name: "var_short", short: 1)
let var_number = CleverTap.sharedInstance()?.defineVar(name: "var_number", number: NSNumber(value: 32))
let var_long = CleverTap.sharedInstance()?.defineVar(name: "var_long", long: 64)
// Dictionary
let var_dict = CleverTap.sharedInstance()?.defineVar(name: "var_dict", dictionary: [
      "nested_string": "hello, nested",
      "nested_double": 10.5
    ])

let var_dict_nested = CleverTap.sharedInstance()?.defineVar(name: "var_dict_complex", dictionary: [
        "nested_int": 1,
        "nested_string": "hello, world",
        "nested_map": [
            "nested_map_int": 15,
            "nested_map_string": "hello, nested map",
        ]
    ])
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

// Primitive types
CTVar *var_string = [[CleverTap sharedInstance] defineVar:@"var_string" withString:@"hello, world"];
CTVar *var_int = [[CleverTap sharedInstance] defineVar:@"var_int" withInt:10];
CTVar *var_bool = [[CleverTap sharedInstance] defineVar:@"var_bool" withBool:YES];
CTVar *var_float = [[CleverTap sharedInstance] defineVar:@"var_float" withFloat:6.0];
CTVar *var_double = [[CleverTap sharedInstance] defineVar:@"var_double" withDouble:60.999];
CTVar *var_short = [[CleverTap sharedInstance] defineVar:@"var_short" withShort:1];
CTVar *var_number = [[CleverTap sharedInstance] defineVar:@"var_number" withNumber:[[NSNumber alloc] initWithInt:32]];
CTVar *var_long = [[CleverTap sharedInstance] defineVar:@"var_long" withLong:64];
// Dictionary
CTVar *var_dict = [[CleverTap sharedInstance] defineVar:@"var_dict" withDictionary:@{
  @"nested_string": @"hello, nested",
  @"nested_double": @10.5
  }];
CTVar *var_dict_nested = [[CleverTap sharedInstance] defineVar:@"var_dict_complex" withDictionary:@{
  @"nested_int": @1,
  @"nested_string": @"hello, world",
  @"nested_map": @{
    @"nested_map_int": @15,
    @"nested_map_string": @"hello, nested map",
  }
}];
```

## Define File Type Variables

CleverTap supports file type variables starting from CleverTap iOS SDK v7.0.0 and above. Supported file types include but are not limited to images (jpg, jpeg, png, gif), text files, and PDFs.

You can define a new file type variable using the following code:

```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

CTVar *var_file = [[CleverTap sharedInstance] defineFileVar:@"fileVariable"];
```
```swift Swift
let var_file = CleverTap.sharedInstance()?.defineFileVar(name: "fileVariable")
```

# Sync Defined Variables

After defining your variables in the code, you must send/sync variables to the server. From the CleverTap dashboard, mark a required CleverTap user profile as a test profile. For more information, refer to [Mark a User Profile as a Test Profile](https://docs.clevertap.com/docs/user-profiles#mark-a-user-profile-as-a-test-profile).

After marking the profile as a test profile, you must sync the app variables in DEBUG mode:

```swift Swift
// 1. Define CleverTap variables 
// …
// 2. Add variables/values changed callbacks
// …

// 3. Sync CleverTap Variables from DEBUG mode/builds
CleverTap.sharedInstance()?.syncVariables();
```
```objectivec
// 1. Define CleverTap variables 
// …
// 2. Add variables/values changed callbacks
// …

// 3. Sync CleverTap Variables from DEBUG mode/builds
[[CleverTap sharedInstance] syncVariables];
```

<Callout icon="📘" theme="info">
  ### Key Points to Remember

  - If another user profile has already created a draft in the dashboard, the sync call will fail to avoid overwriting important changes. In such a case, you should either Publish or Dismiss the existing draft before proceeding with syncing variables again. However, if you have created a draft previously using the sync method, you can override this draft to enhance the integration experience.
    - You may receive the following console logs from the CleverTap SDK:
      - Variables synced successfully.
      - Unauthorised access from a non-test profile. To address this, mark the profile as a test profile from the CleverTap dashboard.
</Callout>

# Fetch Variables During a Session

During a session, you can fetch the updated values for your CleverTap variables from the server. If variables have changed, the appropriate callbacks are triggered. The callback returns a boolean flag that indicates if the update was successful. The callback is triggered regardless of whether the variables have changed or not.

```swift Swift
CleverTap.sharedInstance()?.fetchVariables({ success in
    print(success)
})
```
```objectivec
[[CleverTap sharedInstance] fetchVariables:^(BOOL success) {
        
}];
```

# Use Fetched Variables Values

This process involves the following three major steps:

1. [Fetch Variable Values](doc:ios-remote-config#fetch-variable-values) .
2. [Verify Status of Fetch Variables Request](doc:ios-remote-config#verify-status-of-fetch-variables-request) .
3. [Access Variable Values](doc:ios-remote-config#access-variable-values) .

## Fetch Variable Values

Variables are updated automatically when server values are received. Use the individual callback to receive feedback when a specific variable is updated.

```swift Swift
variable?.onValueChanged {
    print("variable.onValueChanged: \(variable?.value ?? "")")
}
```
```objectivec
[variable onValueChanged:^{
    NSLog(@"variable.onValueChanged: %@", [variable value]);
}];
```

## Verify Status of Fetch Variables Request

The `fetchVariables` method helps verify whether the variables are successfully retrieved from the server.

```swift Swift
CleverTap.sharedInstance()?.fetchVariables({ success in
    print(success)
})
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

[[CleverTap sharedInstance] fetchVariables:^(BOOL success) {   
}];
```

## Re-evaluate Segments After Property Update

When implementing Product Experience segments that depend on user property changes, you must update the user properties first and then immediately fetch variables to ensure the segment evaluation reflects the latest user data.

If a user property (for example, Customer Type) is used in Product Experience segments, you must re-fetch variables after updating that property. This ensures the user sees the correct app experience.

Follow this sequence to ensure that user property changes trigger Product Experience re-evaluation:

1. Update user properties using any of the [supported method](https://developer.clevertap.com/docs/user-profiles-ios#updating-user-profilepush-profile-method).
2. Call `fetchVariables()` to trigger re-evaluation of segments using the updated data.

### Example Re-evaluation

```swift
// Update user property
let profile: Dictionary<String, AnyObject> = [
    "Customer Type": "Premium" as AnyObject
]
CleverTap.sharedInstance()?.profilePush(profile)

// Immediately fetch variables to re-evaluate segments
CleverTap.sharedInstance()?.fetchVariables({ success in
    if success {
        // Product Experiences will now use the updated "Customer Type" property
        // Access your variables here or rely on existing variable callbacks
    }
})
```
```objectivec
// Update user property
NSDictionary *profile = @{
    @"Customer Type": @"Premium"
};
[[CleverTap sharedInstance] profilePush:profile];

// Immediately fetch variables to re-evaluate segments
[[CleverTap sharedInstance] fetchVariables:^(BOOL success) {
    if (success) {
        // Product Experiences will now use the updated "Customer Type" property
        // Access your variables here or rely on existing variable callbacks
    }
}];
```

<Callout icon="📘" theme="info">
  ### Note

  - Always call `fetchVariables()` immediately after any user property updates used in Product Experience segments.
  - The fetch call is asynchronous; use the callback to confirm success.
  - Existing `VariableCallback` instances are automatically triggered when variables change.
</Callout>

## Access Variable Values

You can access the fetched values in the following ways:

- [From `Var` Instance](doc:ios-remote-config#from-var-instance)
- [Using `CleverTap` Instance Method](doc:ios-remote-config#using-clevertap-instance-method)

### From `Var` Instance

You can use several methods on the `Var` instance, as shown in the following code. For File Variables, value returns the file downloaded path.

```swift Swift
// Variables
let var_string = CleverTap.sharedInstance()?.defineVar(name: "var_string", string: "hello, world")
var_string?.defaultValue // returns default value
var_string?.value // returns current value
var_string?.numberValue // returns value as NSNumber if applicable
var_string?.stringValue // returns value as String

// File Variables
let var_file = CleverTap.sharedInstance()?.defineFileVar(name: "fileVariable")
var_file?.value // returns file downloaded path
var_file?.stringValue // returns file downloaded path
var_file?.fileValue // returns file downloaded path
```
```objectivec
// Variables
CTVar *var_string = [[CleverTap sharedInstance] defineVar:@"var_string" withString:@"hello, world"];
var_string.defaultValue; // returns default value
var_string.value; // returns current value
var_string.numberValue; // returns value as NSNumber if applicable
var_string.stringValue; // returns value as String

// File Variables
CTVar *var_file = [[CleverTap sharedInstance] defineFileVar:@"fileVariable"];
var_file.value // returns file downloaded path
var_file.stringValue // returns file downloaded path
var_file.fileValue // returns file downloaded path
```

### Using `CleverTap` Instance Method

You can use the `CleverTap` instance method to get the current value of a variable. If the variable is non-existent, the method returns `null`.

```swift Swift
CleverTap.sharedInstance()?.getVariableValue("variable name")
```
```objectivec
[[CleverTap sharedInstance]getVariableValue:@"variable name"];
```

# Set Up Callbacks

CleverTap iOS SDK provides several callbacks for the developer to receive feedback from the SDK. You can use them as per your requirement, using all of them is not mandatory. They are as follows:

- All Variables Callbacks
- Individual Variable Callback
- Variables Delegate
- All File variables Callback
- File variables individual Callback
- File variables Delegates

## All Variables Callbacks

### `onVariablesChanged`

This callback is invoked when variables are initialized with values fetched from the server. It is called each time new values are fetched.

```swift Swift
let var_string = CleverTap.sharedInstance()?.defineVar(name: "myString", string: "hello,world")
CleverTap.sharedInstance()?.onVariablesChanged {
    print("CleverTap.onVariablesChanged: \(var_string?.value ?? "")")
}
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

CTVar *var_string = [[CleverTap sharedInstance] defineVar:@"var_string" withString:@"hello, world"];
    [[CleverTap sharedInstance] onVariablesChanged:^{
        NSLog(@"CleverTap.onVariablesChanged: %@", [var_string value]);
    }];
```

### `onceVariablesChanged`

This callback is invoked when variables are initialized with values fetched from the server. It is called only once.

```swift Swift
let var_string = CleverTap.sharedInstance()?.defineVar(name: "myString", string: "hello,world")
CleverTap.sharedInstance()?.onceVariablesChanged {
    print("CleverTap.onceVariablesChanged: \(var_string?.value ?? "")")
}
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

CTVar *var_string = [[CleverTap sharedInstance] defineVar:@"var_string" withString:@"hello, world"];

[[CleverTap sharedInstance] onceVariablesChanged:^{
  // Executed only once
  NSLog(@"CleverTap.onceVariablesChanged: %@", [var_string value]);
}];
```

## Individual Variable Callback

### `onValueChanged`Callback

This individual callback is registered per variable. This callback is invoked when the value of the variable changes.

```swift Swift
let var_string = CleverTap.sharedInstance()?.defineVar(name: "myString", string: "hello,world")
var_string?.onValueChanged {
    print("var_string.onValueChanged: \(var_string?.value ?? "")")
}
```
```objectivec
CTVar *var_string = [[CleverTap sharedInstance] defineVar:@"var_string" withString:@"hello, world"];
[var_string onValueChanged:^{
  NSLog(@"var_string.onValueChanged: %@", [var_string value]);
}];
```

## Variables Delegate

The `CTVarDelegate` method is implemented to be invoked when the variable value is changed.

```swift
@objc class VarDelegateImpl: NSObject, VarDelegate {
    func valueDidChange(_ variable: CleverTapSDK.Var) {
        print("CleverTap \(String(describing: variable.name)):valueDidChange to: \(variable.value!)")
    }
}

var_string?.setDelegate(self)
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

@interface CTVarDelegateImpl : NSObject <CTVarDelegate>
@end


@implementation CTVarDelegateImpl
- (void)valueDidChange:(CTVar *)variable {
// valueDidChange
}
@end

CTVarDelegateImpl *del = [[CTVarDelegateImpl alloc] init];
[var_string setDelegate:del];
```

## All File Variables Callback

### `onVariablesChangedAndNoDownloadsPending`

This callback will be called when no files need to be downloaded or all downloads have been completed. It is called each time new values are fetched, and downloads are completed.

```swift
CleverTap.sharedInstance()?.onVariablesChangedAndNoDownloadsPending {
   // Executed each time        
}
```
```objectivec
[[CleverTap sharedInstance] onVariablesChangedAndNoDownloadsPending:^{
  // Executed each time
}];
```

### `onceVariablesChangedAndNoDownloadsPending`

This callback will also be called when no files need to be downloaded, or all downloads have been completed, but It is called only once.

```swift
CleverTap.sharedInstance()?.onceVariablesChangedAndNoDownloadsPending {
	// Executed only once            
}
```
```objectivec
[[CleverTap sharedInstance] onceVariablesChangedAndNoDownloadsPending:^{
  // Executed only once
}];
```

## File variables individual Callback

### `onFileIsReady`

This callback will be called when the value of the file variable is downloaded and ready. This is only available for File variables.

```swift
let var_file = CleverTap.sharedInstance()?.defineFileVar(name: "fileVariable")

var_file?.onFileIsReady {
  // Called when file is downloaded.          
}
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

CTVar *var_file = [[CleverTap sharedInstance] defineFileVar:@"fileVariable"];
[var_file onFileIsReady:^{
	  // Called when file is downloaded.
}];
```

## File Variables Delegates

The `fileIsReady` method is called when file is downloaded. This method is only for file type variables and variable's value will return the file downloaded path.

```swift
@objc class VarDelegateImpl: NSObject, VarDelegate {
    func valueDidChange(_ variable: CleverTapSDK.Var) {
        print("CleverTap \(String(describing: variable.name)):valueDidChange to: \(variable.value!)")
    }
    
    func fileIsReady(_ variable: CleverTapSDK.Var) {
        print("CleverTap file downloaded to path: \(variable.value ?? "nil")")
    }
}

var_file?.setDelegate(self)
```
```objectivec
#import <CleverTapSDK/CleverTap+CTVar.h>

@interface CTVarDelegateImpl : NSObject <CTVarDelegate>
@end


@implementation CTVarDelegateImpl
- (void)valueDidChange:(CTVar *)variable {
// valueDidChange
}

- (void)fileIsReady:(CTVar *)var {
    NSLog(@"CleverTap file var:%@ is downloaded at path: %@", var.name ,var.value);
}
@end

CTVarDelegateImpl *del = [[CTVarDelegateImpl alloc] init];
[var_file setDelegate:del];
```

## Fetch A/B Experiment Variants

From CleverTap iOS SDK v7.4.1 onwards, you can retrieve the active A/B experiment variants for the current user. This is useful when you need to access variant data for custom implementations or analytics, independent of Remote Config variables.

<Callout icon="📘" theme="info">
  ### SDK Prerequisite

  Before you proceed, check that you are using iOS SDK v7.4.1 or higher to use this method.
</Callout>

The variants method returns a list of the user's active variants. If no experiment variants are active for the current user, variants() returns an empty list.

### Sample response

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

# Troubleshooting

For troubleshooting, enable CleverTap SDK logs and set the log level to [Debug/Verbose](https://developer.clevertap.com/docs/advanced-options-ios#debugging).

## Variables Not Syncing

1. **Check User Profile**: Ensure the user is marked as a Test profile on the Dashboard.
2. **Verify Method Call**: Confirm that the `SyncVariables` method is correctly implemented in your DEBUG build.
3. **Log Analysis**: Review the Console/LogCat for any error messages related to variable syncing.

## Unauthorized Access Error

**Solution**: Double-check that the user is properly marked as a Test profile on the Dashboard.

#### "Existing Draft of Another User" Error

**Cause**: A draft of Variables created by a different user already exists.

**Solution**:

1. Access the dashboard.
2. Publish or dismiss the existing draft.
3. Attempt to sync variables again.

<Callout icon="📘" theme="info">
  ### Note

  You can override a draft you previously created via the sync method.
</Callout>

# Verifying Successful Sync

Check the log message _Variables synced successfully_ in the console output.

### Important Considerations

1. **Variable Definition**: Ensure variables are defined before calling `SyncVariables`.
   - If variables are defined in different parts of the app, navigate through those screens before calling `SyncVariables`.

2. **Instance-Specific**: Variables are defined per CleverTap instance.
   - Use the default/shared instance if you have only one CleverTap instance.

3. **Production Builds**: Remove the `SyncVariables` method call from release/production builds as it will not function in those environments.

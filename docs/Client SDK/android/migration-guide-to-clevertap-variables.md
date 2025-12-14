---
title: Migration Guide for CleverTap Variables
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

This guide equips you with the necessary information and resources to migrate your Variables to CleverTap. A well-planned and executed migration can lead to more effective utilization of the product. As a result, the CleverTap team is dedicated to supporting you in achieving this objective.

> 📘 Private Beta
>
> The Remote Config feature is released in Private Beta, while A/B Tests functionality is being developed and coming soon.

### Useful References

* [User guide](https://docs.clevertap.com/docs/remote-config)
* [Android developer guide](https://developer.clevertap.com/docs/android-remote-config)
* [iOS developer guide](https://developer.clevertap.com/docs/ios-remote-config)

# New Terminology

As we work towards integrating Leanplum Variables and A/B testing capabilities into CleverTap, we will implement some changes in how these features are named. The functionalities under the Leanplum **Variables** page will now be called **Remote Config** in CleverTap. At the same time, the **A/B test** retains its existing name. Both *Remote Config* and *A/B Tests* will now fall under the umbrella term, **Product Experiences**.

Lastly, we will continue to use the term variables to refer to the elements in your app that are defined and synced for remote control.

## Leanplum Structure

* Variables
* A/B Tests

## CleverTap structure:

* Product Experiences
  * Remote Config
  * A/B Tests

## Variable Types

| Data Type  | Compatibility                                                                                                                                                                                                                                                                           |
| :--------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Number     | Supported on CleverTap. No changes needed.                                                                                                                                                                                                                                              |
| Boolean    | Supported on CleverTap. No changes needed.                                                                                                                                                                                                                                              |
| String     | Supported on CleverTap. No changes needed.                                                                                                                                                                                                                                              |
| Dictionary | Supported on CleverTap. No changes needed.                                                                                                                                                                                                                                              |
| Array      | Deprecated, map to String variable with a delimiter. You can use comma-separated values.                                                                                                                                                                                                |
| Color      | Deprecated, change to String variable and map to Hex color, or change to Number and use byte mask for the color.                                                                                                                                                                        |
| File       | Not supported in the current version of Remote Config but coming soon. In case you use File variables, until the File type of variable is introduced, you can re-define them as String variables, host the image on your side and provide the URL as value to these types of variables. |

## Variable structures in code

Earlier Variables were defined as structures in the code to visually and systematically arrange them. There were two ways to structure and organize your variables in the Leanplum Dashboard as shown in the following code:

```Text Using Dictionary variables type
//Example:
let powerUp = Var(name: "powerUp", dictionary: \[  
 "name": "Turbo Boost",  
 "speedMultiplier": 1.5)
```
```Text Using dot notation definitions
//Example:
let text = Var(name: "Title.Text", string: "Title 1")
```

CleverTap continues to support the existing Dictionary and dot notation definitions to ensure a smooth migration process. However, in the long term, we plan to move away from these structures in the code. With the upcoming versions of Remote Config, we plan to introduce a feature that allows users to create folders in the dashboard and move variables inside them. This will simplify the structure of variables in your code and provides the flexibility to organize and modify them in the dashboard.

> 📘 Note
>
> If you are currently using these existing structures, you may consider switching directly to a flat variable structure within your code as part of the migration process. 
>
> Later, you can take advantage of the Dashboard Folders. We recommend switching to a flat structure only for apps that do not have a lot of folders and nesting currently. For more complex structures, the list with variables may become harder to manage before the dashboard folders are introduced and it is better to continue using the current nested structure.

# Steps to Migrate from Leanplum Variables to CleverTap Remote Config:

The following are the six major steps involved in the migration process:

1. Upgrade to the LP Wrapper SDK with support for Remote Config (Variables).
2. Copy your variable definitions to CleverTap syntax while preserving your current Leanplum variables.
3. Sync the variables to the CleverTap dashboard and test them.
4. Introduce a switching mechanism via a variable that determines the variables payload your app must use: either Leanplum or CleverTap.
5. Publish and distribute a version of your app, which still operates with Leanplum variables but has all the verified changes to be able to switch to CleverTap variables.
6. Switch to CleverTap variables payload.

## Step-by-Step Migration

### 1. Upgrade to Wrapper SDK

First, you need to upgrade to the Wrapper SDK version, where the Remote Config (Variables) support is added. Exact Wrapper SDK versions will be specified soon.

### 2. Copy variable definitions using the new CleverTap syntax

To define your variables in CleverTap, you must get the CleverTap instance. This is done using `Leanplum.addCleverTapInstanceCallback()` callback. The callback provides the CleverTap instance that is used by the Leanplum Wrapper SDK. You must use this instance to invoke CleverTap methods.

A variable is defined using the `defineVar()` method, which returns an instance of a `CTVar` variable. For that, you must provide the name and default value of the variable. The name of a variable can contain dot-separated strings, which represent the nesting of the variable inside a group or multiple groups. You can also define Dictionary variables to group the key-value pairs. These dictionary variables behave the same as Leanplum variables

#### For iOS

```swift
let ctCallback = CleverTapInstanceCallback(callback: { CleverTapInstance in  
        // Primitive types  
        let var_string = CleverTapInstance.defineVar(name: "var_string", string: "hello, world")  
        let var_int = CleverTapInstance.defineVar(name: "var_int", integer: 10)  
        let var_bool = CleverTapInstance.defineVar(name: "var_bool", boolean: true)  
        let var_float = CleverTapInstance.defineVar(name: "var_float", float: 6.0)  
        let var_double = CleverTapInstance.defineVar(name: "var_double", double: 60.999)  
        let var_short = CleverTapInstance.defineVar(name: "var_short", short: 1)  
        let var_number = CleverTapInstance.defineVar(name: "var_number", number: NSNumber(value: 32))  
        let var_long = CleverTapInstance.defineVar(name: "var_long", long: 64)  
        // Dictionary  
        let var_dict = CleverTapInstance.defineVar(name: "var_dict", dictionary: [  
                "nested_string": "hello, nested",  
                "nested_double": 10.5  
                ])
                                                      
       let var_dict_nested = CleverTapInstance.defineVar(name: "var_dict_complex", dictionary: [
            "nested_int": 1,
            "nested_string": "hello, world",
            "nested_map": [
            "nested_map_int": 15,
            "nested_map_string": "hello, nested map",
            ]
            ])
       })
Leanplum.addCleverTapInstance(callback: ctCallback)
                                               
                                                      
```
```objectivec
  CleverTapInstanceCallback *ctCallback = [[CleverTapInstanceCallback alloc] initWithCallback:^(CleverTap * _Nonnull instance) {
        // Primitive types
    CTVar *var_string = [instance defineVar:@"var_string" withString:@"hello, world"];
    CTVar *var_int = [instance defineVar:@"var_int" withInt:10];
    CTVar *var_bool = [instance defineVar:@"var_bool" withBool:YES];
    CTVar *var_float = [instance defineVar:@"var_float" withFloat:6.0];
    CTVar *var_double = [instance defineVar:@"var_double" withDouble:60.999];
    CTVar *var_short = [instance defineVar:@"var_short" withShort:1];
    CTVar *var_number = [instance defineVar:@"var_number" withNumber:[[NSNumber alloc] initWithInt:32]];
    CTVar *var_long = [instance defineVar:@"var_long" withLong:64];
    // Dictionary
    CTVar *var_dict = [instance defineVar:@"var_dict" withDictionary:@{
        @"nested_string": @"hello, nested",
        @"nested_double": @10.5
    }];
  CTVar *var_dict_nested = [instance defineVar:@"var_dict_complex" withDictionary:@{
            @"nested_int": @1,
            @"nested_string": @"hello, world",
            @"nested_map": @{
                @"nested_map_int": @15,
                @"nested_map_string": @"hello, nested map",
            }
    }];

    }];
    [Leanplum addCleverTapInstanceCallback:ctCallback];

```

#### For Android

```java
// Using defineVariable method

Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
// Integer variable
Var<Integer> var1 = cleverTapInstance.defineVariable("var1", 1);

// Integer variable in a group
Var<Integer> var2 = cleverTapInstance.defineVariable("group.var2", 1);

// Integer variable in nested groups
Var<Integer> var3 = cleverTapInstance.defineVariable("group1.group2.var3", 1);

Map<String, Object> map = new HashMap<String, Object>() {{
    put("int_var", 1);
    put("str_var", "str");
}};
Var<Map<String, Object>> group = cleverTapInstance.defineVariable("group", map);

});



// Using @Variable annotation on static field
// @Variable annotation should come from com.clevertap.android.sdk.variables.annotations package

public class MyClass {
  @Variable
  public static int var1 = 1;

  // Variable in a group
  @Variable(group = "group")
  public static int var2 = 2;

  // Variable in nested group
  @Variable(group = "group1.group2")
  public static int var3 = 3;
}


// You need to tell CleverTap SDK that you have such definition by using:

Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
  cleverTapInstance.parseVariablesForClasses(MyClass.class);
});


// Using @Variable annotation on instance field
// @Variable annotation should come from com.clevertap.android.sdk.variables.annotations package


public class MyClass {
  @Variable
  public int var1 = 1;

  // Variable in a group
  @Variable(group = "group")
  public int var2 = 2;

  // Variable in nested group
  @Variable(group = "group1.group2")
  public int var3 = 3;
}


// You need to tell CleverTap SDK that you have such definition by using:
MyClass instance = new MyClass();

Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
  cleverTapInstance.parseVariables(instance);
});

```
```kotlin
// Using defineVariable method

Leanplum.addCleverTapInstanceCallback { cleverTapInstance -> 
           / Integer variable
val var1: Var<Int> = cleverTapInstance.defineVariable("var1", 1)

// Integer variable in a group
val var2: Var<Int> = cleverTapInstance.defineVariable("group.var2", 1)

// Integer variable in nested groups
val var3: Var<Int> = cleverTapInstance.defineVariable("group1.group2.var3", 1)

// Group with two variables inside

Map<String, Object> map = new HashMap<String, Object>() {{
    put("int_var", 1);
    put("str_var", "str");
}};
Var<Map<String, Object>> group = cleverTapInstance.defineVariable("group", map);

}

// Using @Variable annotation is currently not supported in Kotlin, neither in LP nor in CT


```

> 🚧 Note
>
> While defining your CleverTap variables you should not remove your current Leanplum defined variables as you will be able to use them in parallel during the migration in order to test and validate the new implementаtion.
>
> Now is the time to clean-up unused variables that you may be preserving due to older app versions support. Copy to CleverTap only variables that you are still using. Users on your older app versions with Leanplum SDK will continue to fetch your variables from Leanplum dashboard, so you can safely remove them while transferring to CleverTap variables which will be fetched only in your newest app versions.

### 3. Sync Variables to CleverTap Dashboard Using New Approach

Now that all your CleverTap variables are defined, you can easily sync them to your CleverTap dashboard and start your testing. Use the `Leanplum.addCleverTapInstanceCallback` callback to get the instance and follow the instructions to sync the Variables:

```swift Swift
let ctCallback = CleverTapInstanceCallback(callback: { CleverTapInstance in  
// Define your Variables first and then sync them
// Call syncVariables only once after defining them
CleverTapInstance.syncVariables()   
 })
Leanplum.addCleverTapInstance(callback: ctCal)
```
```Text Java
Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
cleverTapInstance.syncVariables() });

```

* [Android Sync Variables](https://developer.clevertap.com/docs/android-remote-config#sync-defined-variables)
* [iOS Sync Variables](https://developer.clevertap.com/docs/ios-remote-config#sync-defined-variables)

#### 3.1 Implement Callbacks

Next, you need to Implement the CleverTap callbacks in the same places where you are using the Leanplum ones.\
The callbacks use a CleverTap Instance. Use the Leanplum.addCleverTapInstanceCallback callback to get the instance. Follow the Developer Guides, to check on the available callbacks.

* [iOS Setup Callbacks](https://developer.clevertap.com/docs/ios-remote-config#setup-callbacks)
* [Android Setup Callbacks](https://developer.clevertap.com/docs/android-remote-config#set-up-callbacks)

#### 3.2 Mid-session fetch

You can fetch your variables and update them to the most recent values from the server mid-session using the CleverTap `fetchVariables` method. It works the same way as Leanplum `forceContentUpdate`. Use the `Leanplum.addCleverTapInstanceCallback` callback to get the instance.

#### For iOS

```swift
let ctCallback = CleverTapInstanceCallback(callback: { CleverTapInstance in
 CleverTapInstance.fetchVariables({ success in
    print(success)
})
})
Leanplum.addCleverTapInstance(callback: ctCallback)
```
```objectivec
Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
cleverTapInstance.fetchVariables(new FetchVariablesCallback() {
    @Override
    public void onVariablesFetched(boolean isSuccess) {
        // isSuccess is true when server request is successful, false otherwise
    }
});

})

```

#### For Android

```java
Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
cleverTapInstance.fetchVariables(new FetchVariablesCallback() {
    @Override
    public void onVariablesFetched(boolean isSuccess) {
        // isSuccess is true when server request is successful, false otherwise
    }
});

})
```
```kotlin
Leanplum.addCleverTapInstanceCallback { cleverTapInstance -> 

cleverTapInstance.fetchVariables { isSuccess ->
  // isSuccess is true when server request is successful, false otherwise
}
}
```

### 4. Using both Leanplum and CleverTap Variables

When migrating, both Leanplum and CleverTap variables will be available as data sources for your app. In this way, there can be a simultaneous usage period during which you can start running Campaigns in the CleverTap dashboard but still use Leanplum to control variables until you decide to switch over to CleverTap variables payload.

At this step, your code should already have definitions for both Leanplum and CleverTap variables. You can continue using the Leanplum variables like you have been doing so far. You can use CleverTap variables through the CleverTap instance. Add Leanplum.addCleverTapInstanceCallback callback to get the instance.

If you want to switch remotely to either Leanplum or CleverTap. In that case, you can define a boolean variable in Leanplum and CleverTap to use as a flag. We recommend this approach as it allows you to control which stream of variables you use without the need to release a new app version.

```swift
/ LP flag
var useLPVars = Var(name: "useLPFlag", boolean: false)

let ctCallback = CleverTapInstanceCallback(callback: { CleverTapInstance in
// CT flag
        let useCTVars = CleverTapInstance.defineVar(name: "useCTVars", boolean: false)
})
Leanplum.addCleverTapInstance(callback: ctCallback)

//Based on the values of those variables, you can decide which Variables to use, those from Leanplum or those from CleverTap.

If useLPVars.boolValue() {
// use Leanplum
} else if useCTVars.boolValue() {
// use CleverTap
}




```

> 🚧 Note
>
> We cannot migrate the already running A/B tests from Leanplum to CleverTap. For a seamless user experience, we recommend completing your already running A/B tests, applying the desired variable overrides to the Variables dashboard, and transferring them to the CleverTap dashboard. If you preserve running A/B tests on Leanplum when migrating all your variables to CleverTap, users that upgrade to your newest app version will stop seeing the variable values from the respective A/B test variant they were in and start receiving the CleverTap payload instead, which will defeat the purpose of the A/B test.

## Variables Control During Migration Process

| Activity                                                                                                                                                                                                                                                                                                                                                                                   | Variables control                                                                                                                          |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Upgrade to Wrapper SDK                                                                                                                                                                                                                                                                                                                                                                  | Leanplum dashboard for all app versions                                                                                                    |
| 2. Copy your variable definitions to CleverTap syntax while preserving your current Leanplum variables 3. Sync your variables to the CleverTap dashboard & test them 4. Introduce a switching mechanism via a variable that determines which variables payload your app should use - Leanplum or CleverTap. 5. Publish and distribute a production version of the app with the Wrapper SDK | - Leanplum dashboard for all live app versions - CleverTap dashboard for the dev build you’re working on                                   |
| 6. Switch to CleverTap variables payload                                                                                                                                                                                                                                                                                                                                                   | - Leanplum dashboard for users on all older app versions without the Wrapper SDK - CleverTap dashboard for users on the newest app version |

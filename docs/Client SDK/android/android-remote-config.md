---
title: Android Product Experiences
excerpt: Learn how to set up Remote Config for your Android application.
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

CleverTap's Remote Config feature enables you to modify your app's functionality and appearance without pushing updates through the App Store or Google Play using variables. The CleverTap SDK provides the ability to create variables on the client side that can be updated with new values from the server. Additionally, variables can be utilized in A/B tests to test features for a percentage of users selectively.

To define variables, developers can use the platform-specific call within CleverTap SDK. After defining the variables in the code, you must sync the variables to the dashboard through the provided SDK method.

<Callout icon="📘" theme="info">
  ### SDK Prerequisite

  Before you proceed, check that you are using Android SDK v5.0.0 or higher to use the Remote Config feature.
</Callout>

# Supported Variable Types

Currently, CleverTap SDK supports the following variable types:

- boolean
- byte
- short
- int
- long
- float
- double
- String
- File (supported for CleverTap SDK **v7.0.0 and above**)

# Define Variables

One can define variables using the following three methods:

- Using the `defineVariable(...)` method.
- Using the `@Variable` annotation on a static field of a class
- Using the `@Variable` annotation on an instance field of a class

## Use`defineVariable(...)` Method

When calling the `defineVariable(...)` method, you can provide the variable's name and default value.

```java
Var<Byte> var1 = cleverTap.defineVariable("var_byte", (byte) 1);

Var<Short> var2 = cleverTap.defineVariable("var_short", (short) 2);

Var<Integer> var3 = cleverTap.defineVariable("var_int", 3);

Var<Long> var4 = cleverTap.defineVariable("var_long", 4L);

Var<Float> var5 = cleverTap.defineVariable("var_float", 5F);

Var<Double> var6 = cleverTap.defineVariable("var_double", 6.);

Var<String> var7 = cleverTap.defineVariable("var_string", "str");

Var<Boolean> var8 = cleverTap.defineVariable("var_boolean", true);

```
```kotlin kotlin
val var1: Var<Byte> = cleverTap.defineVariable("var_byte", 1)

val var2: Var<Short> = cleverTap.defineVariable("var_short", 2)

val var3: Var<Int> = cleverTap.defineVariable("var_int", 3)

val var4: Var<Long> = cleverTap.defineVariable("var_long", 4L)

val var5: Var<Float> = cleverTap.defineVariable("var_float", 5F)

val var6: Var<Double> = cleverTap.defineVariable("var_double", 6.0)

val var7: Var<String> = cleverTap.defineVariable("var_string", "str")

val var8: Var<Boolean> = cleverTap.defineVariable("var_boolean", true)
  
```

## Use `@Variable` Annotation on a Static Variable

You can use the `@Variable` annotation when defining a static variable; however, the class field must be publicly accessible. After defining the variables, you must instruct the CleverTap Android SDK to parse that specific class by calling the `parseVariablesForClasses()` method.

```java
public class MyClass {
  @Variable
  public static boolean var_boolean = true;

  @Variable
  public static byte var_byte = 1;

  @Variable
  public static short var_short = 2;

  @Variable
  public static int var_int = 3;

  @Variable
  public static long var_long = 4L;

  @Variable
  public static float var_float = 5F;

  @Variable
  public static double var_double = 6.;

  @Variable
  public static String var_string = "str";
}

// You must instruct CleverTap SDK that you have such definition by using:
cleverTap.parseVariablesForClasses(MyClass.class);

```
```kotlin Kotlin
// Using the @Variable annotation in Kotlin files is not supported yet.
// It will be added in a future release.

```

<Callout icon="🚧" theme="warn">
  ### Proguard

  It is mandatory to skip your classes from Proguard when using the `@Variable` annotation because changing the names of the fields leads to runtime errors

  ```groovy
  -keep class com.package.MyClass {
      *;
  }
  ```
</Callout>

## Use `@Variable` Annotation on Instance Variable

You can use the annotation `@Variable` when defining an instance variable; however, the class field must be publicly accessible. After defining the variables, you must instruct the CleverTap Android SDK to parse that specific instance variable using the `parseVariables(instance)`.  This method for parsing the variables is different from the `parseVariablesForClasses()` method and takes the instance as an argument.

```java
public class MyClass {
  @Variable
  public boolean var_boolean = true;

  @Variable
  public byte var_byte = 1;

  @Variable
  public short var_short = 2;

  @Variable
  public int var_int = 3;

  @Variable
  public long var_long = 4L;

  @Variable
  public float var_float = 5F;

  @Variable
  public double var_double = 6.;

  @Variable
  public String var_string = "str";
}

// You must instruct CleverTap SDK that you have such a definition by using:
MyClass instance = new MyClass();
cleverTap.parseVariables(instance);
```
```java Kotlin
// Using the @Variable annotation in Kotlin files is not supported yet.
// It will be added in a future release.
```

<Callout icon="❗️" theme="error">
  ### Proguard

  It is mandatory to skip your classes from Proguard when using the `@Variable` annotation, because changing the names of the fields lead to runtime errors:

  ```groovy
  -keep class com.package.MyClass {
      *;
  }
  ```
</Callout>

## Define File Type Variables

CleverTap supports file type variables starting from CleverTap Android SDK v7.0.0 and above. Supported file types include but are not limited to images (jpg, jpeg, png, gif), text files, and PDFs.

You can define a new file type variable using the following code:

```java Kotlin
val fileVar: Var<String> = cleverTap.defineFileVariable("var_file")
```
```java
Var<String> fileVar = cleverTap.defineFileVariable("var_file");
```

<Callout icon="📘" theme="info">
  ### Note

  Using the `@Variable` annotation for file type variables is not currently supported.
</Callout>

# Sync Defined Variables

After defining your variables in the code, you must send/sync variables to the server. From the CleverTap dashboard, mark a required CleverTap user profile as a test profile. For more information, refer to [Mark a User Profile as a Test Profile](https://docs.clevertap.com/docs/user-profiles#mark-a-user-profile-as-a-test-profile).

After marking the profile as a test profile, you must sync the app variables in DEBUG mode:

```java Kotlin
cleverTap.syncVariables()

```
```java
cleverTap.syncVariables();
```

<Callout icon="📘" theme="info">
  ### Key Points to Remember

  - In a scenario where a draft is already created by another user profile in the dashboard, the sync call will fail to avoid overriding important changes made by someone else. In this case, Publish or Dismiss the existing draft before you proceed with syncing variables again. However, you can override a draft you created via the sync method previously to optimize the integration experience.
  - You can receive the following Logcat logs from the CleverTap SDK:
    - Variables synced successfully.
    - Unauthorized access from a non-test profile. To address this, mark the profile as a test profile from the CleverTap dashboard.
</Callout>

# Fetch Variables During a Session

During a session, you can fetch the updated values for your CleverTap variables from the server. If variables have changed, the appropriate callbacks are triggered. The callback returns a boolean flag that indicates if the update was successful. The callback is triggered regardless of whether the variables have changed or not.

```java Kotlin
cleverTap.fetchVariables { isSuccess ->
  // isSuccess is true when server request is successful, false otherwise
}
```
```java
cleverTap.fetchVariables(new FetchVariablesCallback() {
    @Override
    public void onVariablesFetched(boolean isSuccess) {
        // isSuccess is true when server request is successful, false otherwise
    }
});
```

# Use Fetched Variables Values

This process involves the following three major steps:

1. [Fetch variable values](doc:android-remote-config#fetch-variable-values)  .
2. [Verify the status of Fetch variables request](doc:android-remote-config#verify-status-of-fetch-variables-request).
3. [Access variable values](doc:android-remote-config#access-variable-values) .

## Fetch Variable Values

Variables are updated automatically when server values are received. Use the individual callback to receive feedback when a specific variable is updated.

```java Kotlin
variable.addValueChangedCallback(object : VariableCallback<Int>() {
  override fun onValueChanged(varInstance: Var<Int>) {
    // invoked on app start and whenever value is changed
  }
})

```
```java
variable.addValueChangedCallback(new VariableCallback() {
    @Override
    public void onValueChanged(Var varInstance) {
        // invoked on app start and whenever value is changed
    }
});

```

## Verify Status of Fetch Variables Request

The `FetchVariablesCallback()` method helps verify whether the variables are successfully retrieved from the server.

```java Kotlin
cleverTap.fetchVariables { isSuccess ->
  // isSuccess is true when server request is successful, false otherwise
}
```
```java
cleverTap.fetchVariables(new FetchVariablesCallback() {
    @Override
    public void onVariablesFetched(boolean isSuccess) {
        // isSuccess is true when server request is successful, false otherwise
    }
});
```

<br />

## Re-evaluate Segments After Property Update

When implementing Product Experience segments that depend on user property changes, you must update the user properties first and then immediately fetch variables to ensure the segment evaluation reflects the latest user data.

If a user property (for example, Customer Type) is used in Product Experience segments, you must re-fetch variables after updating that property. This ensures the user sees the correct app experience.

Follow this sequence to ensure that user property changes trigger Product Experience re-evaluation:

1. Update user properties using any of the [supported method](https://developer.clevertap.com/docs/user-profiles-android#update-the-user-profile).
2. Call `fetchVariables()` to trigger re-evaluation of segments using the updated data.

### Example Re-evaluation

```kotlin
// Update user property
val profileUpdate = HashMap<String, Object>()
profileUpdate.put("Customer Type", "Premium")
clevertapDefaultInstance.pushProfile(profileUpdate)

// Immediately fetch variables to re-evaluate segments
cleverTap.fetchVariables { isSuccess ->
    if (isSuccess) {
        // Product Experiences will now use the updated "Customer Type" property
        // Access your variables here or rely on existing variable callbacks
    }
}
```
```java
// Update user property
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Customer Type", "Silver");
clevertapDefaultInstance.pushProfile(profileUpdate);

// Immediately fetch variables to re-evaluate segments
cleverTap.fetchVariables(new FetchVariablesCallback() {
    @Override
    public void onVariablesFetched(boolean isSuccess) {
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

You can access the fetched values in the following three ways:

- [From Annotated @Variable field](doc:android-remote-config#from-annotated-variable-field)
- [From `Var` instance](doc:android-remote-config#from-var-instance)
- [Using `CleverTapAPI` Class](doc:android-remote-config#use-clevertapapi-class)

### From Annotated @Variable field

Class fields annotated by `@Variable` are automatically updated, and when you use a field, it will have the latest server value.

### From `Var` Instance

You can use several methods on the `Var` instance, as shown in the following code:

```java Kotlin
variable.defaultValue() // returns default value
variable.value() // returns current value
variable.numberValue() // returns value as Number if applicable
variable.stringValue() // returns value as String

```
```java
variable.defaultValue(); // returns default value
variable.value(); // returns current value
variable.numberValue(); // returns value as Number if applicable
variable.stringValue(); // returns value as String

```

### Use `CleverTapAPI` Class

You can use the `CleverTapAPI` class to get the current value of a variable. If the variable is nonexistent, the method returns `null`.

```java Kotlin
cleverTap.getVariableValue(“variable name”)
```
```java
cleverTap.getVariableValue(“variable name”);
```

# Set Up Callbacks

CleverTap Android SDK provides several callbacks for developers to receive feedback from the SDK. You can use them per your requirement; using all of them is not mandatory. They are as follows:

- Change in individual variable value.
- Initialize or change all variables.
- Individual Callback
- Change all variable values.
  - File for a variable is downloaded and ready.
  - Change in variable value and no download pending.

<Callout icon="📘" theme="info">
  ### Retrieving Variables

  The retrieval of variables is limited to a single callback, meaning subsequent calls to the method overwrite the previous callback internally. As a result, only the most recent callback is triggered when the variables are retrieved.
</Callout>

## Change Individual Variable Value

The individual callback is registered per variable. It is called when the app starts or whenever the variable value changes.

<br />

```java Kotlin
val variable: Var<Int> = cleverTap.defineVariable("var_int", 1)
variable.addValueChangedCallback(object : VariableCallback<Int>() {
  override fun onValueChanged(varInstance: Var<Int>) {
    // invoked on app start and whenever value is changed
  }
})
```
```java
Var variable = cleverTap.defineVariable("var_int", 1);
variable.addValueChangedCallback(new VariableCallback() {
    @Override
    public void onValueChanged(Var varInstance) {
        // invoked on app start and whenever value is changed
    }
});
```

## Initialize or Change All Variables

The global callback registered on the CleverTap instance is called when variables are initialized with a value or changed with a new server value.

```java Kotlin
// invoked on app start and whenever vars are fetched from server
cleverTap.addVariablesChangedCallback(object : VariablesChangedCallback() {
  override fun variablesChanged() {
    // implement
  }
})

// invoked only once on app start, or when added if server values are already received
cleverTap.addOneTimeVariablesChangedCallback(object : VariablesChangedCallback() {
  override fun variablesChanged() {
    // implement
  }
})

```
```java
// invoked on app start and whenever vars are fetched from the server
cleverTap.addVariablesChangedCallback(new VariablesChangedCallback() {
    @Override
    public void variablesChanged() {
        // implement
    }
});
// invoked only once on app start, or when added if server values are already received
cleverTap.addOneTimeVariablesChangedCallback(new VariablesChangedCallback() {
    @Override
    public void variablesChanged() {
        // implement
    }
});

```

## Individual Callback

This individual callback is registered per file variable. It is called when the file associated with the variable is downloaded and ready to be used.

```java Kotlin
val fileVar: Var<String> = cleverTap.defineFileVariable("var_file")
fileVar.addFileReadyHandler(object : VariableCallback<String>() {
    override fun onValueChanged(variable: Var<String>) {
        // invoked when file corresponding to this variable is downloaded and ready to be used
    }
})

```
```java
Var<String> fileVar = cleverTap.defineFileVariable("var_file");
fileVar.addFileReadyHandler(new VariableCallback<String>() {
    @Override
    public void onValueChanged(Var<String> variable) {
        // invoked when file corresponding to this variable is downloaded and ready to be used
    }
});

```

<Callout icon="🚧" theme="warn">
  ### Note

  You might have to call the`fetchVariables()` method again after registering variable if they are not registered at app startup.
</Callout>

## Change All Variable Values

This global callback registered on the CleverTap instance is called when variable values are changed, and the associated files are downloaded and ready to be used.

### Callback triggered each time new values are fetched and downloaded

```java Kotlin
cleverTap.onVariablesChangedAndNoDownloadsPending(object : VariablesChangedCallback() {
    override fun variablesChanged() {
        // Will be called each time new values are fetched and download is completed
    }
})

```
```java
cleverTapAPI.onVariablesChangedAndNoDownloadsPending(new VariablesChangedCallback() {
    @Override
    public void variablesChanged() {
        // Will be called each time new values are fetched and download is completed
    }
});

```

### Callback Triggered Only Once When New Values Are Fetched and Downloaded

```java Kotlin
cleverTap.onceVariablesChangedAndNoDownloadsPending(object : VariablesChangedCallback() {
    override fun variablesChanged() {
        // Will be called only once when new values are fetched and download is completed
    }
})

```
```java
cleverTapAPI.onceVariablesChangedAndNoDownloadsPending(new VariablesChangedCallback() {
    @Override
    public void variablesChanged() {
        // Will be called only once when new values are fetched and download is completed
    }
});

```

<Callout icon="🚧" theme="warn">
  ### Note

  These callbacks guarantee that all variables, except file type variables, are initialized.
</Callout>

## Fetch A/B Experiment Variants

From CleverTap Android SDK v7.7.1 onwards, you can retrieve the active A/B experiment variants for the current user. This is useful when you need to access variant data for custom implementations or analytics, independent of Remote Config variables.

<Callout icon="📘" theme="info">
  ### SDK Prerequisite

  Before you proceed, check that you are using Android SDK v7.7.1 or higher to use this method.
</Callout>

The variants() method returns a list of the user's active variants. If no experiment variants are active for the current user, variants() returns an empty list.

### Sample response:

```java
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

For troubleshooting, enable CleverTap SDK logs and set the log level to [Debug/Verbose](https://developer.clevertap.com/docs/android-advanced-features#debugging).

## Common Issues and Solutions

### Variables Not Syncing

1. **Check User Profile**: Ensure the user is marked as a Test profile on the Dashboard.
2. **Verify Method Call**: Confirm that the `SyncVariables` method is correctly implemented in your DEBUG build.
3. **Log Analysis**: Review the Console/LogCat for any error messages related to variable syncing.

### Unauthorized Access Error

**Solution**: Double-check that the user is properly marked as a Test profile on the Dashboard.

### "Existing Draft of Another User" Error

**Cause**: A draft of Variables created by a different user already exists.

**Solution**:

1. Access the dashboard.
2. Publish or dismiss the existing draft.
3. Attempt to sync variables again.

<Callout icon="📘" theme="info">
  ### Note

  You can override a draft you previously created via the sync method.
</Callout>

## Verify Successful Sync

Look for the log message "Variables synced successfully" in the console output.

### Important Considerations

- **Variable Definition**: Ensure variables are defined before calling `SyncVariables`.<br />If variables are defined in different parts of the app, navigate through those screens before calling SyncVariables.
- **Instance-Specific**: Variables are defined per CleverTap instance. Use the default/shared instance if you have only one CleverTap instance.
- **Production Builds**: Remove the `SyncVariables` method call from release/production builds as it will not function in those environments.

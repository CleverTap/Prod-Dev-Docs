---
title: React Native Custom Code In-App Templates
excerpt: >-
  Learn how to create and manage Custom Code Templates for In-App messages using
  the CleverTap React Native SDK.
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

With CleverTap React Native SDK 3.1.0 and above, the CleverTap platform supports **Custom Code In-App message presentations**, enabling developers to utilize In-App notifications with tailored configurations and presentation logic. The SDK provides the following two types of customizable components:

- **Templates**
- **Functions**

## Key Differences Between Templates and Functions

- **Templates** can include action arguments, whereas **Functions** cannot.
- **Functions** can be used as actions; **Templates** cannot.  
  Functions can be set as either Visual or Non-Visual in the Builder. 
  - **Visual Functions** can contain UI logic and will be part of the [In-App queue](https://developer.clevertap.com/docs/react-native-custom-code-in-app-templates#in-app-queue). 
  - **Non-Visual Functions** are triggered directly when invoked without UI logic.

## Creating Templates and Functions

Templates and Functions are defined in JSON files placed in the appropriate directories. Each template includes a name, type, and set of arguments. Functions additionally specify whether they are visual or non-visual.

The template builders validate the definitions and raise a `CustomTemplateException`if the template is invalid.  
It is generally advised not to handle these exceptions but to fix the definitions instead.

### JSON Structure for Templates and Functions

```json
{
  "TemplateName": {
    "type": "template",
    "arguments": {
      "Argument1": {
        "type": "string|number|boolean|file|action|object",
        "value": "val"
      },
      "Argument2": {
        "type": "object",
        "value": {
          "Nested1": {
            "type": "string|number|boolean|object",
            "value": "val"
          },
          "Nested2": {
            "type": "string|number|boolean|object",
            "value": "val"
          }
        }
      }
    }
  },
  "functionName": {
    "type": "function",
    "isVisual": true|false,
    "arguments": {
      "a": {
        "type": "string|number|boolean|file|object",
        "value": "val"
      }
    }
  }
}
```

The JSON definitions should be placed in one or more files in the following locations:

- **Android**: In `assets` directory
- **iOS**: In any directory linked to the project

Optionally the `react-native-assets` package can be utilized to keep both files in sync.

## Arguments

Arguments are structures that represent the configuration of custom code templates. It consists of a type and a value. The supported argument types are:

- **Primitives**: Boolean, Number, String (with default values).
- **Object**:  An object where keys are argument names and values are arguments with supported primitive values.
- **File**: A file that will be downloaded when the template is triggered.
- **Action**: A built-in action (such as, close, open URL) or a function.

### Hierarchical Arguments

Arguments can be grouped in two ways:

1. Using an object argument.
2. Indicating the group in the argument's name with a "." symbol.

Both methods are treated the same. However, `file` and `action`type arguments can only be grouped by specifying the group in their names.

#### Example: Identical Hierarchical Definitions

Using an object:

```json
"arguments": {
  "map": {
    "type": "object",
    "value": {
      "a": {
        "type": "number",
        "value": 5
      },
      "b": {
        "type": "number",
        "value": 6
      }
    }
  }
}
```

Using dot notation:

```json
"arguments": {
  "map.a": {
    "type": "number",
    "value": 5
  },
  "map.b": {
    "type": "number",
    "value": 6
  }
}
```

## Template Definition Example

```json
"ExampleTemplate": {
  "type": "template",
  "arguments": {
    "boolArg": {
      "type": "boolean",
      "value": false
    },
    "stringArg": {
      "type": "string",
      "value": "Default"
    },
    "mapArg": {
      "type": "object",
      "value": {
        "int": {
          "type": "number",
          "value": 0
        },
        "string": {
          "type": "string",
          "value": "Default"
        }
      }
    },
    "actionArg": {
      "type": "action"
    },
    "fileArg": {
      "type": "file"
    }
  }
}
```

## Registering Custom Templates

Custom templates must be registered within the native applications.

### Android

Register templates in the `Application.onCreate` method using `CleverTapCustomTemplates.registerCustomTemplates`.

- If you are extending `CleverTapApplication`, add the following line before calling `super.onCreate()`:

```java
public class MainApplication extends CleverTapApplication {
    @Override
    public void onCreate() {
        CleverTapCustomTemplates.registerCustomTemplates(this, "templateDefinitionsFileInAssets.json");
        super.onCreate();
    }
}
```

- If you are not extending `CleverTapApplication`, add the line before calling `CleverTapRnAPI.initReactNativeIntegration(this)`:

```java
public class MainApplication extends Application {
    @Override
    public void onCreate() {
        ActivityLifecycleCallback.register(this);
        super.onCreate();
        CleverTapCustomTemplates.registerCustomTemplates(this, "templateDefinitionsFileInAssets.json");
        CleverTapRnAPI.initReactNativeIntegration(this);
    }
}
```

### iOS

Register custom templates by calling `[CleverTapReactCustomTemplates registerCustomTemplates]` in your  `AppDelegate.didFinishLaunchingWithOptions` method, before invoking `[CleverTap autoIntegrate]`.

```objc
@implementation AppDelegate
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [CleverTapReactCustomTemplates registerCustomTemplates:@"templates", nil];
  [CleverTap autoIntegrate];
  [[CleverTapReactManager sharedInstance] applicationDidLaunchWithOptions:launchOptions];

  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}
```

## Sync In-App Templates to Dashboard

The templates must be synced to the CleverTap dashboard to use in campaigns.  
Use the following methods for syncing:

- **Debug Builds**: Call `CleverTap.syncCustomTemplates()` or `CleverTap.syncCustomTemplatesInProd()` when the SDK user is marked as a test user.
- **Release Builds**: Remove the sync method to prevent unintended changes.

## Presenting Custom Templates

When a custom template is triggered, the following events are raised:  

- **`CleverTap.CleverTapCustomTemplatePresent`**  
- **`CleverTap.CleverTapCustomFunctionPresent`**  

Applications using custom templates or functions must subscribe to these events using `CleverTap.addListener`. The event data will include the template's name.  

### Managing Template States

Once the event handler is invoked, the template is considered "active" until `CleverTap.customTemplateSetDismissed(templateName)` is called. While a template is "active," the following functions can be used:  

1. **Obtain Argument Values**  
   Use `customTemplateGet*Arg(templateName: string, argName: string)` to retrieve argument values.  

2. **Trigger Actions**  
   Execute actions by their name with `CleverTap.customTemplateRunAction(templateName: string, argName: string)`.  

3. **Set Template State**  
   - `CleverTap.customTemplateSetPresented(templateName: string)`: Indicates the template is displayed to the user.  
   - `CleverTap.customTemplateSetDismissed(templateName: string)`: Indicates the template is no longer displayed.  

### Display Rules

- Only **one visual template** or **In-App message** can be displayed simultaneously.  
- New messages are blocked until the current one is dismissed.  

### Handling Template Closure

Applications should also subscribe to the`CleverTap.CleverTapCustomTemplateClose`. This event is raised when a template should be closed (e.g. if an action of type "close" is triggered).  

**Steps for Handling Closure:**  

1. Remove the UI associated with the template.  
2. Call `CleverTap.customTemplateSetDismissed(templateName)` to notify the SDK that the template is closed.

### Example Usage

```js
CleverTap.addListener(CleverTap.CleverTapCustomTemplatePresent, templateName => {
    // show the UI for the template and call customTemplateSetDismissed when it is closed
    showTemplateUi({
        template: templateName,
        onClose: ()=> {
            CleverTap.customTemplateSetDismissed(templateName);
        }
    });
    // call customTemplateSetPresented when the UI has become visible to the user
   CleverTap.customTemplateSetPresented(templateName);
});

CleverTap.addListener(CleverTap.CleverTapCustomTemplateClose, templateName => {
    // close the corresponding UI before calling customTemplateSetDismissed
    CleverTap.customTemplateSetDismissed(templateName);
});
```

## In-App Queue

When an In-App message is triggered, it is added to a queue based on its priority and displayed only after all previous messages have been dismissed. This queue is saved in the storage and persists across app launches, ensuring messages are shown whenever possible. Custom code In-App messages follow the same logic—they are triggered only when their corresponding notification is next in the queue. However, the next In-App message will not be displayed until the current template invokes the `CleverTapPlugin.customTemplateSetDismissed(templateName)` method.

## File Downloading and Caching

File arguments are downloaded automatically when the template is triggered. If a file fails to download, the corresponding template is skipped.

**File downloading occurs**

- **Client-Side Templates**: During app launch or just before template presentation.
- **Server-Side Templates**: Immediately before template presentation.
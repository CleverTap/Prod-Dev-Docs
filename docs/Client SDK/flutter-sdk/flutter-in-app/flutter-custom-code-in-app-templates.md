---
title: Flutter Custom Code In-App Templates
excerpt: >-
  Learn how to create and manage Custom Code Templates for In-App messages using
  the CleverTap Flutter SDK.
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

With Flutter Plugin SDK 3.0.0 and above, CleverTap supports the custom display of In-App messages. This feature lets developers customize the configuration and presentation logic for In-App notifications. The SDK supports the following two types of components:

* **Templates**
* **Functions**

## Key Differences Between Templates and Functions

* **Templates** can include action arguments, while **Functions** cannot.  
* **Functions** can be used as actions, whereas **Templates** cannot. Functions can be categorized as:  
  * **Visual Functions**: These contain UI logic and are part of the [In-App queue](https://developer.clevertap.com/docs/flutter-custom-code-in-app-templates#in-app-queue).  
  * **Non-Visual Functions**: These are triggered directly upon invocation without any UI logic.

# Create Templates and Functions

Templates and Functions are defined in JSON files placed in the appropriate directories. Each template includes a name, type, and set of arguments. Functions additionally specify whether they are visual or non-visual.

The template builders validate the definitions and raise a `CustomTemplateException`if the template is invalid.\
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

The JSON definitions must be placed in one or more files located in the following directories:

* **Android**: In `assets` directory.
* **iOS**: In any directory linked to the project.

## Argument

Arguments are structures that represent the configuration of custom code templates. It consists of a type and a value. The supported argument types are:

* **Primitives**: Boolean, Number, String (with default values).
* **Object**:  An object where keys are argument names and values are arguments with supported primitive values.
* **File**: A file that will be downloaded when the template is triggered.
* **Action**: A built-in action (for example, close, open URL) or a function.

## Hierarchical Arguments

Arguments can be grouped in two ways:  

1. Using an **Object Argument**  
2. Using a **Dot (.) in Argument Names**

Both approaches are treated equally.

**Example Using an Object Argument**

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

**Example Using a Dot Symbol**

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

# Register Custom Code Templates

Templates must be registered before the SDK can process them.

### Android

1. Call `CleverTapCustomTemplates.registerCustomTemplates` in the `onCreate()` method **before** calling `super.onCreate()`:

```java
public class MainApplication extends CleverTapApplication {
    @Override
    public void onCreate() {
        ActivityLifecycleCallback.register(this);
        CleverTapCustomTemplates.registerCustomTemplates(this, "templates.json");
        super.onCreate();
    }
}
```

### iOS

1. Call `[CleverTapPluginCustomTemplates registerCustomTemplates]` in `AppDelegate.didFinishLaunchingWithOptions`method **before** calling `[CleverTap autoIntegrate]`:

```objc
@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [CleverTapPluginCustomTemplates registerCustomTemplates:@"templates", nil];
    [GeneratedPluginRegistrant registerWithRegistry:self];
    [[CleverTapPlugin sharedInstance] applicationDidLaunchWithOptions:launchOptions];
    [CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option
    return [super application:application didFinishLaunchingWithOptions:launchOptions];
}
```

# Sync In-App Templates to Dashboard

To use the templates in campaigns, they must be synced to the CleverTap dashboard.\
Use the following methods for syncing:

* **Debug Builds**: Call `CleverTap.syncCustomTemplates()` or `CleverTap.syncCustomTemplatesInProd()` when the SDK user is marked as a *test user*. 
* **Release Builds**: Remove the sync method to prevent unintended changes.

# Present Templates

When a custom template is triggered, the corresponding event `CleverTap.CleverTapCustomTemplatePresent` or `CleverTap.CleverTapCustomFunctionPresent` will be raised. Applications with custom templates or functions must subscribe to these events through the`setCleverTapCustomTemplatePresentHandler` or, where the event data will be the template's name.

**Example**:

```coffeescript Javascript
// Subscribe to the custom template present event
document.addEventListener('CleverTap.CleverTapCustomTemplatePresent', function (event) {
    // Log the template name when the event is triggered
    console.log('Template name:', event.detail);
});
```

When the event handler is invoked, the template will be considered *active* until `CleverTapPlugin.customTemplateSetDismissed(templateName)` is called. While a template is *active*, the following functions can be used:

* Obtain argument values by using the appropriate `customTemplateGet*Arg(templateName: string, argName: string)` methods.
* Trigger actions by their name through `CleverTapPlugin.customTemplateRunAction(templateName: string, argName: string)`.
* Set the state of the template invocation. `CleverTap.customTemplateSetPresented(templateName: string)` and `CleverTap.customTemplateSetDismissed(templateName: string)` notify the SDK of the state of the current template invocation. The *presented* state means the In-App is displayed to the user, and the *dismissed* state means the In-App is no longer visible.

> 📘 Single Visual Template Limitation
>
> The SDK can only display one visual template or In-App message at a time. New messages cannot be shown until the current message is dismissed.

Applications should also subscribe to the `setCleverTapCustomTemplateCloseHandler` listener, which will be raised when a template should be closed (e.g., when an action of type "close" is triggered). Use this listener to remove the UI associated with the template and call `CleverTap.customTemplateSetDismissed(templateName)` to close it.

### Example of Template Presenter

```js
void activateCleverTapFlutterPluginHandlers() {
  _clevertapPlugin.setCleverTapCustomTemplatePresentHandler(presentCustomTemplate);
  _clevertapPlugin.setCleverTapCustomTemplateCloseHandler(closeCustomTemplate);
  _clevertapPlugin.setCleverTapCustomFunctionPresentHandler(presentCustomFunction);
}

void presentCustomTemplate(String templateName) async {
  // some function which shows ui
  showDialog(
    data: Data,
    onClose: closeCustomTemplate(templateName),
    onPresented: setCustomTemplatePresented(templateName)
  );
}

void setCustomTemplatePresented(String templateName) {
  CleverTapPlugin.customTemplateSetPresented(templateName);
}

void closeCustomTemplate(String templateName) {
  CleverTapPlugin.customTemplateSetDismissed(templateName);
}

void presentCustomFunction(String templateName) {
  CleverTapPlugin.customTemplateSetPresented(templateName);
}
```

# In-App Queue

When an In-App message is triggered, it is added to a queue based on its priority and displayed only after all previous messages have been dismissed. This queue is saved in the storage and persists across app launches, ensuring messages are shown whenever possible. Custom code In-App messages follow the same logic—they are triggered only when their corresponding notification is next in the queue. However, the next In-App message will not be displayed until the current template invokes the `CleverTapPlugin.customTemplateSetDismissed(templateName)` method.

# File Downloading and Caching

File arguments are downloaded automatically when the template is triggered. If a file fails to download, the corresponding template is skipped.  

#### File downloading occurs

* **Client-Side Templates**: During app launch or just before template presentation.  
* **Server-Side Templates**: Immediately before template presentation.

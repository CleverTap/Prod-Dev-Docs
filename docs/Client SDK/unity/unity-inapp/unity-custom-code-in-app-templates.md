---
title: Unity Custom Code In-App Templates
excerpt: >-
  Learn how to create and manage Custom Code Templates for In-App messages using
  the CleverTap Unity SDK.
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

With Unity Plugin SDK 5.0.0 and above, CleverTap supports the custom display of In-App messages. This feature lets developers customize the configuration and presentation logic for In-App notifications. The SDK supports the following two types of components:

* **Templates**  
* **Functions**

## Key Differences Between Templates and Functions

* **Templates** can include action arguments, while **Functions** cannot.
* **Functions** can be used as actions, whereas **Templates** cannot. Functions can be categorized as:
  * **Visual Functions**: These contain UI logic and are part of the [In-App queue](doc:unity-custom-code-in-app-templates#in-app-queue).
  * **Non-Visual Functions**: These are triggered directly upon invocation without UI logic.

# Create Templates and Functions

Create templates and functions by defining a name, type, and arguments (and isVisual for functions). Type and arguments (and isVisual for functions) are required, and names must be unique across the application. The application validates template definitions for correctness at launch. If an invalid template is found, the application raises an exception and closes with a message explaining the error. Always launch the application after making template changes to ensure the definitions are valid and can be triggered correctly.

### Templates are defined in JSON format using the following scheme:

```asp JSON
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

Place JSON definitions in one or more files under `Assets/CleverTap/CustomTemplates`. These files are copied to exported projects and registered automatically with the SDK.

* **Android**: Definitions are stored in `clevertap-android-wrapper.androidlib/assets/CleverTap/CustomTemplates` and registered via `CleverTapUnityAPI.initialize(Context context)`. If `CleverTapUnityApplication` is used. This process is automatic.
* **iOS**: Definitions are copied to `CleverTap/CustomTemplates`, added to the `Unity-iPhone` target, and included in the Copy Bundle Resources build phase. Templates are registered via the `CleverTapUnityAppController.mm`.\
  For reference, see the working example in `CTExample/Assets/CleverTap/templates.json`.

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

> 📘 Grouping Restriction:
>
> `file` and `action `arguments cannot be grouped under hierarchical structures. They must be defined separately.

### Example Using an Object Argument

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

### Example Using a Dot Symbol

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

# Sync In-App Templates to Dashboard

Sync templates with the CleverTap dashboard for use in campaigns. This works only in debug builds and requires the SDK user to be a test user. Use the Following Methods for Syncing:

* **Debug Builds**: Call `CleverTap.SyncCustomTemplates()` while developing templates.
* **Release Builds**: Remove this function to avoid unintended changes.

# Presenting Templates

When a custom template is triggered, CleverTap invokes either `CleverTap.OnCustomTemplatePresent` or `CleverTap.OnCustomFunctionPresent`. Applications using custom templates or functions must subscribe to these events.

The event handler receives a `CleverTapTemplateContext`, which interacts with the template. The template remains **active** until `CleverTapTemplateContext.SetDismissed()` is called. While a template is *active*, the following functions can be used:

* Obtain argument values by using the appropriate `Get*(string argName)` methods.
* Trigger actions by their name through `TriggerAction(string argName)`.
* Set the state of the template invocation. `SetPresented()` and `SetDismissed()` notify the SDK of the state of the current template invocation. The *presented* state means the In-App is displayed to the user, and the *dismissed* state means the In-App is no longer visible.

> 📘 Single Visual Template Limitation
>
> The SDK can only display one visual template or In-App message at a time. New messages cannot be shown until the current message is dismissed.

Applications should also subscribe to the `CleverTap.OnCustomTemplateClose` listener, which will be raised when a template should be closed (For example, when an action of type "close" is triggered). Use this listener to remove the UI associated with the template and call `CleverTapTemplateContext.SetDismissed()` to close it.

### Example of Template Presenter

```csharp
CleverTap.OnCustomTemplatePresent += (CleverTapTemplateContext context) =>
{
    // Show the UI for the template, be sure to keep the context as long as the template UI
    // Is being displayed so that context.setDismissed() can be called when the UI is closed.
    ShowTemplateUi(context);
    // call customTemplateSetPresented when the UI has become visible to the user
   context.SetPresented();
};

CleverTap.OnCustomTemplateClose += (CleverTapTemplateContext context) =>
{
    // Close the corresponding UI before calling customTemplateSetDismissed
    context.SetDismissed();
};
```

# In-App Queue

When an In-App message is triggered, it is added to a queue based on its priority and displayed only after all previous messages have been dismissed. This queue is saved in the storage and persists across app launches, ensuring messages are shown whenever possible. Custom code In-App messages follow the same logic—they are triggered only when their corresponding notification is next in the queue. However, the next In-App message will not be displayed until the current template invokes the `CleverTapTemplateContext.SetDismissed()` method.

# File Downloading and Caching

File arguments are downloaded automatically when the template is triggered. If a file fails to download, the corresponding template is skipped.

### File downloading occurs

* **Client-Side Templates**: During app launch or just before template presentation. If a file download fails, it will retry at the next app launch.
* **Server-Side Templates**: Immediately before template presentation. Failed downloads will prevent the template from triggering.
* If any required file fails to download, the entire template is skipped, and the next In-App message in the queue is shown.

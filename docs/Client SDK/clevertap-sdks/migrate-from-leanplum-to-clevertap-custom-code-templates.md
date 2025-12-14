---
title: Migrate Custom Code Templates
excerpt: >-
  Learn more about migrating custom code In-App templates from Leanplum to
  CleverTap.
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

This document will help you transition templates and functions from Leanplum to CleverTap. It includes creating and managing templates, highlighting differences between the two platforms, and outlining the steps for a successful migration. This document enables users to fully leverage CleverTap's features.

> 📘 Note
> 
> For detailed steps on how to implement the Custom Code Templates in CleverTap, refer to the following docs:
> 
> - [Custom In-App Templates in iOS](doc:ios-custom-code-in-app-templates)
> - [Custom In-App Templates in Android](doc:android-custom-code-in-app-templates)

# Naming Convention

The following table maps the Leanplum naming convention with the CleverTap naming convention, enabling a seamless transition from Leanplum to CleverTap:

| Leanplum Naming Convention | CleverTap Naming Convention |
| :------------------------- | :-------------------------- |
| Custom Templates           | Custom Code Templates       |
| Actions                    | App Functions               |

# Leanplum Vs. CleverTap Logic

This section highlights the key differences between Custom Code Templates in LeanPlum and CleverTap, providing a clear comparison of their functionalities.

| Key Differences | Leanplum Logic                                                                                                                                                            | CleverTap Logic                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| :-------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Template types  | Messages, Actions, or both.                                                                                                                                               | <li> You can define Custom Code templates as either Templates or App Functions.  </li> <li> Templates can have action arguments, which can be selected on the CleverTap dashboard. </li>                                                                                                                                                                                                                                                                                                                                                                                                               |
| Definitions     | Leanplum defines templates and actions using the `defineAction method (name, kind, args, present, dismiss)` or by implementing the `MessageTemplate` (android) interface. | CleverTap defines templates and app functions using separate builders.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Builders        | <ul>   <li>Template name is passed as argument to the `defineAction`.</li>   <li>The arguments are passed to `defineAction` as an array.</li> </ul>                       | <ul>     <li>Builders must set both a name and a presenter.</li>     <li>The presenter serves as the interface, similar to Leanplum’s present and dismiss handlers.</li>     <li>Template or app function arguments are defined using the builders. Template builders validate the correctness of template definitions. If an invalid template is created, the following exception is thrown: NSException on iOS and CustomTemplateException on Android.</li> . <li>It is generally advised not to handle these exceptions; template definitions must be valid to be triggered correctly.</li>   </ul> |

Let’s now understand how template actions are implemented in Leanplum and CleverTap.

## Define a Sample Template

### Leanplum

In Leanplum, a template action is defined using the `Leanplum.defineAction` method, which takes several parameters, including the action name, action kind (in this case, a message), a list of arguments (such as a default text string and an accept action), and handlers for presentation and dismissal of the message. This method allows developers to customize how messages are presented and dismissed within their app, offering developers flexibility in user engagement within their app.

```swift swift
Leanplum.defineAction(name: "Template Name", kind: .message,
                              args: [
                                ActionArg.init(name: "string", string: "Default Text"),
ActionArg.init(name: "Accept action", action: nil)],
                              present: presentHandler,
                              dismiss: dismissHandler)

 
```
```kotlin
Leanplum.defineAction(
    "Template Name",
    ACTION_KIND_MESSAGE,
    ActionArgs()
        .with("string", "Default Text")
        .withAction("Accept action", null),
    presentHandler,
    dismissHandler
)
```

### CleverTap

For CleverTap, the template action definition is illustrated through two examples, one in Kotlin and the other in Swift, reflecting its support for Android and iOS platforms, respectively.

#### For Android Platform

```kotlin
template {
   name("Template Name")
   presenter(presenter)
   stringArgument("string", "Default Text")
   actionArgument("Accept action")
}
```
```java Java
new CustomTemplate.TemplateBuilder()
               .name("Template Name")
               .presenter(presenter)
               .stringArgument("string", "Default Text")
               .fileArgument("file")
               .intArgument("int", 0)
               .build();
```

#### For iOS Platform

```swift
let templateBuilder = CTInAppTemplateBuilder()
templateBuilder.setName("Template Name")
templateBuilder.setPresenter(presenter)
templateBuilder.addArgument("string", string: "Default Text")
templateBuilder.addActionArgument("Accept action")
let template = templateBuilder.build()

 
```

## Arguments

The Leanplum `ActionArg` corresponds to the CleverTap Arguments. The arguments are created using the CleverTap template builder.

| Leanplum-supported Data Types | Compatibility in CleverTap                                                                                               |
| :---------------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| Number                        | Supported on CleverTap. No changes are required.                                                                         |
| Boolean                       | Supported on CleverTap. No changes are required.                                                                         |
| String                        | Supported on CleverTap. No changes are required.                                                                         |
| Dictionary                    | Supported on CleverTap. No changes are required.                                                                         |
| Array                         | Deprecated; instead, use a String argument with a delimiter. You can use comma-separated values.                         |
| Color                         | Deprecated; update to a String argument and map to a Hex color, or switch to a Number and apply byte mask for the color. |
| File                          | Supported on CleverTap. No default value. Use the Leanplum’s default value if the argument is null.                      |

## Present Custom Code Templates in CleverTap

This process can be divided into the following three main steps:

1. Invoke the Presenter
   1. When a custom template is triggered, its presenter is activated.
   2. Presenters must extend either `TemplatePresenter` or `FunctionPresenter` (`CTTemplatePresenter` on iOS) and implement the `onPresent()` method to handle custom UI logic.
2. Handle Template Closure  
   Implement `onClose()` in `TemplatePresenter` to manage the template’s closure. This method should remove the UI and call TemplateContext.setDismissed. This method will remove the UI associated with the template and call `TemplateContext.setDismissed`.
3. Use `CustomTemplateContext` (provided by all presenter methods) to:

- Get argument values using the appropriate `get*(String name)` methods.
- Trigger actions by their name through `CustomTemplateContext.triggerActionArgument`.
- Set the state of the template invocation. Use the `CustomTemplateContext.setPresented` and `CustomTemplateContext.setDismissed` methods to inform the SDK about the current state of the template invocation.
  - The presented state means the In-App is visible to the user.
  - The dismissed state means the In-App is no longer visible.
  - Only one visual template or In-App message can be displayed at a time. New messages can only be shown after the current one is dismissed.

When custom templates are triggered, both Leanplum and CleverTap provide ways to present them to the user. However, the implementation details and available features differ between the two platforms:

| Leanplum                             | CleverTap              |
| :----------------------------------- | :--------------------- |
| presentHandler/present               | onPresent              |
| presentHandler/present return bool   | setPresented(bool)     |
| dismissHandler/dismiss               | onClose                |
| actionContext.actionDismissed()      | context.setDismissed() |
| runActionNamed/runTrackedActionNamed | triggerActionArgument  |

# Migrate Custom Code In-App Templates

Now that we know some of the key differences between CleverTap and Leanplum's Custom Code Template functionality let's learn about the exact steps to successfully migrate Custom Code In-App Templates.

1. Upgrade to the Leanplum Wrapper SDK with support for Custom Code Templates. - 
2. Manually recreate your Custom Template definitions in CleverTap for [iOS](doc:ios-custom-code-in-app-templates#create-templates-and-functions) or [Android](doc:android-custom-code-in-app-templates#create-templates-and-functions). 
3. Register the CleverTap Definitions in [iOS](doc:ios-custom-code-in-app-templates#register-custom-templates) or [Android](doc:android-custom-code-in-app-templates##register-custom-code-in-app-templates). 
4. Sync the templates to the CleverTap dashboard.
5. Now test the Templates and Functions that you have in CleverTap
6. Publish the new version of your app that supports custom templates on both, Leanplum and CleverTap.
7. Migrate your Leanplum campaigns to CleverTap.

## Upgrade to Leanplum Wrapper SDK

Upgrade to the Leanplum Wrapper SDK that supports Custom Code Templates in CleverTap.

## Recreate Custom Template Definitions in CleverTap

Recreate the Custom Template definitions manually in CleverTap. 

## Register CleverTap Definitions

Register the template definitions in CleverTap. The methods to register the custom templates will be called only if a CleverTap instance is created (the migration has been started).

```java Android
CleverTapAPI.registerCustomInAppTemplates
```
```objectivec iOS
CTCustomTemplatesManager.registerTemplateProducer
```

## Sync to CleverTap dashboard

Now that all your CleverTap Custom Templates are registered, you can easily sync them to your CleverTap dashboard and start your testing. Use the `Leanplum.addCleverTapInstanceCallback` callback to get the instance and sync the templates:

```java Android
Leanplum.addCleverTapInstanceCallback(cleverTapInstance -> {
cleverTapInstance.syncRegisteredInAppTemplates() });
```
```objectivec iOS
let ctCallback = CleverTapInstanceCallback(callback: { cleverTapInstance in 
 cleverTapInstance.syncCustomTemplates()
})
Leanplum.addCleverTapInstance(callback: ctCal)
```

## Test the Templates and Functions

The Templates and functions must be tested if they are working correctly in CleverTap. 

## Release a New App Version

Release a new app version that supports Custom Templates and Functions for Leanplum and CleverTap. This will allow you to create new campaigns in CleverTap while keeping the active campaigns in Leanplum. 

> 📘 Note
> 
> Do not remove your Leanplum Custom Templates from the code if any active campaigns in Leanplum are still using them.

## Migrate Leanplum Campaigns

Now you can migrate all your Leanplum campaigns to CleverTap. You can contact your Migration Manager for assistance.
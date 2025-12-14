---
title: Android Custom Code In-App Templates
excerpt: >-
  Learn how to create and manage Custom Code Templates for In-App messages using
  the CleverTap Android SDK.
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

With Android SDK 7.0.0 and above, the CleverTap platform supports displaying Custom Code In-App messages. This feature allows developers to customize how In-App notifications are configured. The SDK provides the following two types of customizable components:

* Templates
* Functions

## Key Differences between Templates and Functions

* **Templates** can include action arguments, while **Functions** cannot.
* **Functions** can be used as actions; **Templates** cannot. Functions can be set as either of the following: 
  * **Visual Functions** can contain UI logic and will be part of the [In-App queue](https://developer.clevertap.com/docs/android-custom-code-in-app-templates#in-app-queue).
  * **Non-Visual Functions** are triggered directly upon invocation without UI logic. 

# Create Templates and Functions

Templates and Functions can be created through **Builders**  supported in SDK's versions 7.0.0 and above. Each template includes a name, a set of arguments, and a presenter. The name and presenter are mandatory, and the template name must be unique within the application. 

The template builders validate the definitions and raise a `CustomTemplateException` if the template is invalid.\
It is generally advised not to handle these exceptions, but to fix the definitions instead.

## Arguments

Arguments are key-value pairs used to configure custom code templates and functions.\
The supported argument types are:

* **Primitives**: Boolean, Byte, Short, Integer, Long, Float, Double, String. Each primitive must have a default valuet hat will be used if no other value is specified.
* **Map**: A collection of supported primitives containing keys as argument names. 
* **File**: A file argument that is downloaded when the template is triggered.
* **Action**: An argument for an action. It can be a function or a built-in action such as *close* or *open url*.

### Hierarchical Arguments

You can group arguments in two ways:

* By using a map argument
* By Including Group Name in the Argument’s Name With a Dot (.) Symbol

Both methods are treated the same way. 

#### By Using a `map` Argument

```java
Map<String, Object> map = new HashMap<>();
map.put("a", 5);
map.put("b", 6);
builder.mapArgument("map", map);
```
```kotlin
mapArgument(  
   name = "map",  
   value = mapOf (  
       "a" to 5,  
       "b" to 6  
   )  
)
```

#### By Including Group Name in Argument’s Name With a Dot (.) Symbol

```java
builder.intArgument("map.a", 5); 
builder.intArgument("map.b", 6);
```
```kotlin
builder.intArgument("map.a", 5)  
builder.intArgument("map.b", 6)
```

> 📘 File and Action Type Arguments
>
> * File and Action arguments cannot be used inside a Map.
> * To include a File or Action argument in a group, the group name must be included in the argument's name, separated by a dot (.).

In case you want to add File or Action type arguments to a Map you have defined, you can do it using the dot symbol.

```java
builder.fileArgument("map.fileArg"); 
builder.actionArgument("map.actionArg");
```
```kotlin
builder.fileArgument("map.fileArg")
builder.actionArgument("map.actionArg")
```

## Example of Template and Function Creation

The following is an example code for creating Template and Function:

```java
new CustomTemplate.TemplateBuilder()  
               .name("template")  
               .presenter(presenter)  
               .stringArgument("string", "Default Text")  
               .fileArgument("file")  
               .intArgument("int", 0)  
               .build();

```
```kotlin
template {  
   name("template")  
   presenter(presenter)  
   stringArgument("string", "Default Text")  
   fileArgument("file")  
   intArgument("int", 0)  
}
```

# Register Custom Code In-App Templates

You must register the custom templates before creating the `CleverTapAPI` instance.\
We recommend initializing this instance in the `Application.onCreate()` method.\
If your application uses multiple `CleverTapAPI` instances, use `CleverTapInstanceConfig` to specify the registration of each`CleverTapAPI` instance.

To register custom templates, use the `CleverTapAPI.registerCustomInAppTemplates` method, which accepts a `TemplatesProducer` containing the template definitions.

```java
CleverTapAPI.registerCustomInAppTemplates(ctConfig ->  
   CustomTemplatesExtKt.templatesSet( // or create a Set<CustomTemplate> and add your templates  
       new CustomTemplate.TemplateBuilder()  
           .name("template")  
           .presenter(templatePresenter)  
           .stringArgument("string", "Text")  
           .build(),  
       new CustomTemplate.FunctionBuilder(false) // Check Visual vs Non-visual Functions in "Key Differences" 
           .name("function")  
           .presenter(functionPresenter)  
           .intArgument("int", 0)  
           .build()  
       ));
```
```kotlin
CleverTapAPI.registerCustomInAppTemplates {  
   setOf(  
       template {  
           name("template")  
           presenter(templatePresenter)  
           stringArgument("string", "Text")  
       },  
       function(isVisual = false) {   // Check Visual vs Non-visual Functions in "Key Differences"
           name("function")  
           presenter(functionPresenter)  
           intArgument("int", 0)  
       }  
   )  
}
```

# Sync In-App Templates to Dashboard

Templates must be synced with the CleverTap dashboard to be available in campaigns. First, define and register all templates and functions in the SDK, then use the`CleverTapAPI.syncRegisteredInAppTemplate` method to sync them.

> 🚧 Sync Caution
>
> This sync function should only be used in debug builds with a [test user](https://docs.clevertap.com/docs/user-profiles#mark-a-user-profile-as-a-test-profile). We recommend running this function only during template development and ensure that you remove it before release. Do not include this function in release builds.

## Present Templates

When a custom template is triggered, its presenter is invoked. Depending on the template type, presenters must extend the`TemplatePresenter` or `FunctionPresenter`. Both presenters must implement the `onPresent()` method to execute the template invocation and exhibit their customized UI logic. Additionally, `TemplatePresenter` should implement the `onClose()` method to handle the closing of templates.

All presenter methods provide a `CustomTemplateContext`, that enables you to:

* Obtain argument values with `get*(String name)` methods.
* Trigger actions by their name through `CustomTemplateContext.triggerActionArgument`.
* Set the status of the template invocation using the `CustomTemplateContext.setPresented()` and `CustomTemplateContext.setDismissed()` methods to inform the SDK about the current template invocation. A presented state means that the In-App is displayed to the user. A dismissed state means that the In-App has been dismissed and is no longer visible.

### Example of Template Presenter

Following is an example of Template Presenter:

```java
class MyTemplatePresenter implements TemplatePresenter {
    
    @Override
    public void onPresent(@NonNull CustomTemplateContext.TemplateContext context) {
        // be sure to keep the context as long as the template UI is being displayed
        // so that context.setDismissed() can be called when the UI is closed.
        showUI(context);
        context.setPresented();
    }
    
    @Override
    public void onClose(@NonNull CustomTemplateContext.TemplateContext context) {
        // close the corresponding UI  
        context.setDismissed();
    }
}
```
```kotlin
MyTemplatePresenter: TemplatePresenter {

   override fun onPresent(context: TemplateContext) {  
       // be sure to keep the context as long as the template UI is being displayed  
       // so that context.setDismissed() can be called when the UI is closed.  
       showUI(context)  
       context.setPresented()  
   }

   override fun onClose(context: TemplateContext) {  
       // close the corresponding UI  
       context.setDismissed()  
   }  
}
```

# In-App Queue

When an In-App message is triggered, it is added to a queue based on its priority and displayed only after all previous messages have been dismissed. This queue persists across app launches, ensuring that messages are shown when possible. Custom code In-App messages follow the same logic—they are triggered only when their corresponding notification is next in the queue. However, the next In-App message is not displayed until the current template invokes `CustomTemplateContext.setDismissed()`.

> 📘 Single Visual Template Limitation
>
> The SDK can only display one visual template or In-App message at a time. New messages cannot be shown until the current message is dismissed.

# File Downloading and Caching

File arguments are automatically downloaded and are ready for use when a template is displayed. Files are downloaded when a file argument changes and is not already cached.\
File downloading occurs only before presenting the In-App. If any file argument fails to download, the In-App message is skipped, and the template is not triggered.

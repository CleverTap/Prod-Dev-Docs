---
title: iOS Custom Code In-App Templates
excerpt: >-
  Learn how to create and manage Custom Code Templates for In-App messages using
  the CleverTap iOS SDK.
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

With iOS SDK 7.0.0 and above, the CleverTap platform supports displaying Custom Code In-App messages. This feature allows developers to customize how In-App notifications are configured and displayed. The SDK provides the following two types of customizable components:

- Templates
- Functions

# Key Differences

- **Templates** can include action arguments, while Functions cannot.
- **Functions** can be used as actions; Templates cannot. Functions can be set as either of the following:
  - **Visual Functions** can contain UI logic and can be part of the [In-App queue](https://developer.clevertap.com/docs/ios-custom-code-in-app-templates#in-app-queue).
  - **Non-Visual Functions** are triggered directly upon invocation without UI logic.

# Create Templates and Functions

Templates and Functions can be created through Builders supported in SDK versions 7.0.0 and above.

Each template includes a name, a set of arguments, and a presenter. The name and presenter are mandatory, and each template name must be unique within the application.

The template builders validate the definitions and raise a `NSException` if the template is invalid. It is recommended to address the issues in the template definitions rather than handling these exceptions directly.

## Arguments

Arguments are key-value pairs used to configure custom code templates. The supported argument types are:

- **Primitives**: BOOL, NSString, NSNumber. Each primitive must have a default value to be used if no other value is provided.
- **Dictionary**: A collection of key-value pairs where keys are argument names and values are supported primitive types.
- **File**: A file argument that is downloaded when the template is triggered.
- **Action**: An action argument for an action. It can be a function template or a built-in action such as _close_ or _open url_.

### Hierarchical Arguments

You can group arguments in two ways:

- By Using a `map` Argument
- By Including Group Name in Argument’s Name With a Dot (.) Symbol

> 📘 File and Action Type Arguments
> 
> File and Action arguments are not supported within a Map. The group should be specified in the argument's name to include a File or Action argument in a group.

#### By Using a `map` Argument

```objectivec
   [builder addArgument:@"map" withDictionary:@{
            @"a": @5,
            @"b": @6
        }];

```
```swift
builder.addArgument("map", dictionary: [
   "a": 5,
   "b": 6
])

```

#### By Including Group Name in Argument’s Name With a Dot (.) Symbol

```objectivec
[builder addArgument:@"map.a" withNumber:@5];
[builder addArgument:@"map.b" withNumber:@6];

```
```swift
builder.addArgument("map.a", number: 5)
builder.addArgument("map.b", number: 6)

```

## Example of Template Creation

The following is an example code for creating Template and Function:

```objectivec
CTInAppTemplateBuilder *builder = [CTInAppTemplateBuilder new];
[builder setName:@"template"];
[builder setPresenter:presenter];
[builder addArgument:@"string" withString:@"Default Text"];
[builder addFileArgument:@"file"];
[builder addArgument:@"int" withNumber:@0];
CTCustomTemplate *template = [builder build];
```
```swift
let templateBuilder = CTInAppTemplateBuilder()
templateBuilder.setName("template")
templateBuilder.setPresenter(presenter)
templateBuilder.addArgument("string", string: "Default Text")
templateBuilder.addFileArgument("file")
templateBuilder.addArgument("int", number: 0)
let template = templateBuilder.build()
```

# Register Custom Templates

You must register the custom templates before creating the CleverTap instance. We recommend initializing this instance in the `UIApplicationDelegate application:didFinishLaunchingWithOptions:`. 

If your application uses multiple `CleverTap` instances, use `CleverTapInstanceConfig` to specify the registration of each `CleverTap` instance.

To register custom templates, use the`CTCustomTemplatesManager.registerTemplateProducer ` method, which requires a `CTTemplateProducer`  containing the template definitions.

```objectivec

#import <CleverTapSDK/CTInAppTemplateBuilder.h>
#import <CleverTapSDK/CTTemplateProducer.h>
#import <CleverTapSDK/CTAppFunctionBuilder.h>


@interface TemplateProducer: NSObject<CTTemplateProducer>


@end


@implementation TemplateProducer


- (NSSet<CTCustomTemplate *> * _Nonnull)defineTemplates:(CleverTapInstanceConfig * _Nonnull)instanceConfig {
    CTInAppTemplateBuilder *builder = [CTInAppTemplateBuilder new];
    [builder setName:@"template"];
    [builder setPresenter:presenter];
    [builder addArgument:@"string" withString:@"Default Text"];
    [builder addFileArgument:@"file"];
    [builder addArgument:@"int" withNumber:@0];
    CTCustomTemplate *template = [builder build];
    
    CTAppFunctionBuilder *functionBuilder = [[CTAppFunctionBuilder alloc] initWithIsVisual:YES];
    [functionBuilder setName:@"function"];
    [functionBuilder setPresenter:functionPresenter];
    [functionBuilder addArgument:@"int" withNumber:@0];
    CTCustomTemplate *function = [functionBuilder build];
    
    return [[NSSet alloc] initWithObjects:template, function, nil];
}


@end
```
```swift

class TemplateProducer: CTTemplateProducer {


    public func defineTemplates(_ instanceConfig: CleverTapInstanceConfig) -> Set<CTCustomTemplate> {
        let templateBuilder = CTInAppTemplateBuilder()
        templateBuilder.setName("template")
        templateBuilder.setPresenter(presenter)
        templateBuilder.addArgument("string", string: "Default Text")
        templateBuilder.addFileArgument("file")
        templateBuilder.addArgument("int", number: 0)
        let template = templateBuilder.build()
        
        let functionBuilder = CTAppFunctionBuilder(isVisual: true)
        functionBuilder.setName("function")
        functionBuilder.setPresenter(functionPresenter)
        functionBuilder.addArgument("int", number: 0)
        let function = functionBuilder.build()


        return [template, function]
    }
}
```

# Sync In-App Templates to Dashboard

Templates must be synced with the CleverTap dashboard to be available in campaigns. First, define and register all templates and functions in the SDK, then use the`cleverTapInstance.syncRegisteredInAppTemplates` method to sync them.

> 🚧 Sync Caution
> 
> Remember that this syncing can only be done in debug builds using a test user. We recommend you run this function only during template development and remove it before release. Do not include it in release builds.

## Display Templates

When a custom template is triggered, its presenter is invoked. Depending on the template type, presenters must extend the `CTTemplatePresenter`. Both presenters must implement the `onPresent() `method to invoke the template and exhibit their customized UI logic. Additionally, `TemplatePresenter` must implement the `onClose()` method to handle the closing of templates. This method removes the UI associated with the template and calls the`TemplateContext.setDismissed` method.

All presenter methods provide a `CustomTemplateContext` that enables you to:

- Obtain argument values with `get*(String name)` methods.
- Trigger actions by their name through `CustomTemplateContext.triggerActionArgument`.
- Set the status of the template invocation using the `CustomTemplateContext.setPresented` and `CustomTemplateContext.setDismissed` methods inform the SDK about the current template invocation. A presented state means that the In-App is displayed to the user. A dismissed state means that the In-App has been dismissed and is no longer visible.

### Example of Template Presenter

Following is an example of a Template Presenter:

```objectivec
@interface Presenter : NSObject <CTTemplatePresenter>
@end
@implementation Presenter
- (void)onPresent:(nonnull CTTemplateContext *)context {
    // keep the context as long as the template UI is being displayed
    // so that context.setDismissed() can be called when the UI is closed.
    // showUI()
    [context presented];
}
- (void)onCloseClicked:(nonnull CTTemplateContext *)context {
    // close the corresponding UI
    [context dismissed];
}
@end

```
```swift
class Presenter: CTTemplatePresenter {
    func onPresent(context: CTTemplateContext) {
        // keep the context as long as the template UI is being displayed
        // so that context.setDismissed() can be called when the UI is closed.
        // showUI()
        context.presented()
    }
    
    func onCloseClicked(context: CTTemplateContext) {
        // close the corresponding UI
        context.dismissed()
    }
}
```

## In-App Queue

When an In-App message is triggered, it is added to a queue based on its priority and displayed only after all previous messages have been dismissed. This queue persists across app launches, ensuring that messages are shown when possible. Custom code In-App messages follow the same logic—they are triggered only when their corresponding notification is next in the queue. However, the next In-App message is not displayed until the current template invokes the`CustomTemplateContext.Dismissed()` method.

> 📘 Single Visual Template Limitation
> 
> The SDK can only display one visual template or In-App message at a time. New messages cannot be shown until the current message is dismissed.

## File Downloading and Caching

File arguments are automatically downloaded and are ready for use when a template is displayed. Files are downloaded when a file argument changes and is not already cached. For client-side In-Apps, this happens at app launch and is retried if necessary when the In-App is presented. For server-side In-Apps, file downloading occurs only before presenting the In-App. If any file argument fails to download, the In-App message is skipped, and the template is not triggered.
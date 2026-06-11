---
title: iOS Advanced Features
excerpt: Learn more about advanced iOS features.
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

Enhance your iOS app by integrating advanced CleverTap features such as location tracking, deep link handling, push notifications, debug logging, and SwiftUI support. These integrations help you track user behavior, personalize experiences, and improve engagement.

The SDK also supports [PII Encryption](doc:advanced-options-ios#encryption-of-pii-data) at rest and in transit, ensuring that sensitive user data such as Email, Identity, Name, and Phone is securely stored and transmitted over the network, offering end-to-end data protection.

# Opt Out

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, we provide the following method using which the app can stop sending events to CleverTap.

```swift
CleverTap.sharedInstance()?.setOptOut(true)
```
```objectivec
[[CleverTap sharedInstance] setOptOut:YES];
```

If the user wants to opt in again to share their data, you call the same method like this.

```swift
CleverTap.sharedInstance()?setOptOut(false);
```
```objectivec
[[CleverTap sharedInstance] setOptOut:NO];
```

By default, the SDK will send data to CleverTap, and the opt-out will be set to false. It would be a good practice to capture the opt-out flag of the user and set it whenever the app launches.

# Enable Device Network Info Reporting

By default, the CleverTap iOS SDK does not collect device network-related information, including the IP address, in compliance with data privacy regulations such as GDPR. If your use case requires this data, you can enable tracking explicitly using the following method:

#### Enable Device Network Info Collection

Call this method to allow CleverTap to collect IP address and other network-related metadata.

```swift
CleverTap.sharedInstance()?.enableDeviceNetworkInfoReporting(true)
```
```objectivec
[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:YES];
```

#### Disable Device Network Info Collection

Call this method to prevent CleverTap from collecting any device network information.

```swift
CleverTap.sharedInstance()?.enableDeviceNetworkInfoReporting(false)
```
```objectivec
[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:NO];
```

> 📘 Note
>
> Device network information tracking is disabled by default. Enable it only if necessary for your analytics or compliance use cases.

# Send Location Information to CleverTap

Send the user's location data to CleverTap to enable location-based personalization and analytics.

```swift
func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
    guard let newLocation = locations.last else { return }
    
    // Set location using CleverTap
    CleverTap.setLocation(newLocation.coordinate)
}
```
```objectivec
- (void)locationManager:(CLLocationManager *)manager 
    didUpdateToLocation:(CLLocation *)newLocation 
           fromLocation:(CLLocation *)oldLocation {
    [CleverTap setLocation: newLocation.coordinate];
}
```

# Manually Enable Support for Universal (Deep) Link Tracking

Deep links are a way of launching a native app and providing additional information telling it to do some specific event or show specific content. CleverTap automatically tracks universal links that open your application. If you have universal (deep) links coming to your app, you can capture the incoming UTM parameters easily. Call handleOpenURL:sourceApplication: when the application:openURL:sourceApplication:annotation: message is sent to your app delegate.

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    CleverTap.sharedInstance()?.handleOpen(url, sourceApplication: options[.sourceApplication] as? String)
    return true
}

func open(_ url: URL, options: [UIApplication.OpenExternalURLOptionsKey : Any] = [:],
          completionHandler completion: ((Bool) -> Void)? = nil) {
    CleverTap.sharedInstance()?.handleOpen(url, sourceApplication: nil)
    completion?(true)
}
```
```objectivec
- (BOOL) application:(UIApplication *)application
    openURL:(NSURL *)url
    sourceApplication:(NSString *)sourceApplication
    annotation:(id)annotation {
      [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:sourceApplication];
      return YES;
}

- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options {
    [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:nil];
    return YES;
}

- (void)openURL:(NSURL*)url options:(NSDictionary<NSString *, id> *)options
completionHandler:(void (^ __nullable)(BOOL success))completion {
    [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:nil];
    if (completion) {
        completion(YES);
    }
}
```

# Debugging

By default, CleverTap logs are set to `CleverTapLogLevel.info`. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to `CleverTapLogLevel.debug`. If you want to disable CleverTap logs for production environment, you can set the debug level to `CleverTapLogLevel.off`.

```swift
#if DEBUG
    CleverTap.setDebugLevel(CleverTapLogLevel.debug.rawValue)
#else
    CleverTap.setDebugLevel(CleverTapLogLevel.off.rawValue)
#endif
```
```objectivec
#ifdef DEBUG
   [CleverTap setDebugLevel:CleverTapLogDebug];
#else
   [CleverTap setDebugLevel:CleverTapLogOff];
#endif
```

To debug the device in the CleverTap dashboard and check if events and profiles are being logged successfully on dashboard:

1. Copy the CleverTap ID from the logs.
2. Open the CleverTap dashboard.
3. Navigate to _Segments_ > _Find People_ > search _By Identity_.

<Image align="center" alt={1205} border={true} caption="Search By Identity on CleverTap Dashboard" title="Search By Identity on CleverTap Dashboard" src="https://files.readme.io/fc78aaf-d46c9c8-Android_search_by__identity.png" />

# Setting Account Credentials

If you do not wish to insert your account credentials in your app’s `Info.plist`, or you want to set your account ID programmatically, you can do so in your app delegate, in application:didFinishLaunchingWithOptions:

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {
    CleverTap.setCredentialsWithAccountID("Your account ID here", andToken: "Your account token here")
    CleverTap.autoIntegrate()
    ...
    return true
}
```
```objectivec Objective-C
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [CleverTap setCredentialsWithAccountID:@"Your account ID here" andToken:@"Your account token here"];
    [CleverTap autoIntegrate];
    ...
    return YES;
}
```

If you have set used this method to set your CleverTap Account ID and Token, please ensure that you do not make an entry for these values in your `info.plist` files.

# Checking Push Notifications from CleverTap

If you wish to determine whether a notification originated from CleverTap, call this method:

* (BOOL)isCleverTapNotification:(NSDictionary *)payload;

You must manually call the SDK as follows/add the following CleverTap code to your AppDelegate:

```swift Swift
func userNotificationCenter(_ center: UNUserNotificationCenter, 
                            didReceive response: UNNotificationResponse, 
                            withCompletionHandler completionHandler: @escaping () -> Void) {
        
    /**
     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, 
     the system calls this method with the results. Use this method to perform the task associated with that action. 
     At the end of your implementation, you must call the completionHandler block to let the system know you are done processing.
     */

    let userInfo = response.notification.request.content.userInfo
    print("APPDELEGATE: didReceiveResponseWithCompletionHandler \(userInfo)")

    // Handle CleverTap notification
    if CleverTap.sharedInstance()?.isCleverTapNotification(userInfo) == true {
        CleverTap.sharedInstance()?.handleNotification(withData: userInfo)
    }

    // Handle different actions based on response
    switch response.actionIdentifier {
    case UNNotificationDefaultActionIdentifier:
        print("User tapped on the notification")
    case UNNotificationDismissActionIdentifier:
        print("User dismissed the notification")
    default:
        print("User selected a custom action: \(response.actionIdentifier)")
    }

    // Ensure completionHandler is called to complete the notification response handling
    completionHandler()
}
```
```objectivec
- (void) userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)(void))completionHandler {

    /**
     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.

     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.
     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.

     If you do not implement this method, your app never responds to custom actions.

     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc
     **/

    if ([[CleverTap sharedInstance] isCleverTapNotification:response.notification.request.content.userInfo]) {

       ...
    }
    completionHandler();
}
```

# Encryption of PII data

PII data is stored across the SDK and could contain sensitive information. From CleverTap iOS SDK v5.2.0 onwards, you can enable encryption for PII data such as Email, Identity, Name, Phone, and so on. Additionally, from CleverTap iOS SDK v7.4.0 onwards, you can enable encryption for profile, variables, inbox and event cache data.

CleverTap supports three encryption levels for PII data:

* **None (0)**: Data is stored in plaintext.
* **Medium (1)**: Only PII fields are encrypted.
* **High (2)**: Encrypts all stored data such as profile, variables, inbox and event cache data, not just PII fields. Available from iOS SDK version 7.4.0 and above.

> 📘 Default Encryption Level
>
> By default, the encryption level is set to **None (0)**.

### Set Encryption Level for the Default Instance

The encryption level for the default CleverTap instance on iOS can only be set via the `Info.plist` file. To configure it, add the `CleverTapEncryptionLevel` key of type `String` to the `Info.plist`. Use the value `0` for **None**, `1` for **Medium**, or `2` for **High**.

### Set Encryption Level for Additional Instances

Different instances can have different encryption levels. To set an encryption level for an additional instance:

```swift
// Initialize configuration for an additional CleverTap instance
let ctConfig = CleverTapInstanceConfig.init(accountId: "ADDITIONAL_CLEVERTAP_ACCOUNT_ID", accountToken: "ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN")

// Set encryption level - choose only one

// For No Encryption
ctConfig.encryptionLevel = CleverTapEncryptionLevel.none

// For Medium Encryption
ctConfig.encryptionLevel = CleverTapEncryptionLevel.medium

// For High Encryption (v7.4.0+)
ctConfig.encryptionLevel = CleverTapEncryptionLevel.high

let cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)
```
```objectivec
// Initialize configuration for an additional CleverTap instance
CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@"ADDITIONAL_CLEVERTAP_ACCOUNT_ID" accountToken:@"ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN"];

// Set encryption level - choose only one

// For No Encryption
ctConfig.encryptionLevel = CleverTapEncryptionLevel.none

// For Medium Encryption
[ctConfig setEncryptionLevel:CleverTapEncryptionMedium];

// For High Encryption (v7.4.0+)
[ctConfig setEncryptionLevel:CleverTapEncryptionHigh];

CleverTap *additionalCleverTapInstance = [CleverTap instanceWithConfig:ctConfig];
```

## Encryption in Transit

Encrypting data in transit ensures that sensitive personal data, such as Email, Identity, Name, Phone, and so on, is encrypted before being sent over the network. This protects user data during transmission and adds an extra layer of security beyond the at-rest encryption.

From CleverTap iOS SDK 7.3.1 onwards, this feature is supported natively in the SDK, with encryption and decryption handled automatically.

> 📘 Private Beta
>
> This feature is currently released in Private Beta. To enable it, contact CleverTap Support. Check that it is activated for your account before implementing it in the SDK.

### Encryption Scope

Encryption in transit applies to all event data.

### Prerequisites for Encryption In Transit

To use this feature, ensure that you perform the following steps:

1. Contact your account manager to enable the encryption feature on your account.
2. Set the string key `CleverTapEncryptionInTransitEnabled` to `1` or `0` in your app’s `Info.plist`.

### Encryption Specifications

The following table outlines the encryption specifications used by the CleverTap SDK:

| Parameter      | Value                                |
| -------------- | ------------------------------------ |
| Algorithm      | AES-256 (symmetric)                  |
| IV Size        | 12 bytes (96 bits)                   |
| Encoding       | Base64 for all encrypted content     |
| Key Management | Base64 direct (v0), RSA planned (v1) |

### Error Scenarios

If encryption in transit fails, the SDK returns one of the following error codes:

| Error Code | Error Message                                                   | Description                                                                                                                                                    |
| ---------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `402`      | `"Encryption In Transit disabled. Retry with plain text"`       | The encryption feature is not enabled. To proceed, either enable Encryption In Transit in your CleverTap account or contact your Account Manager to enable it. |
| `419`      | `"Encryption In Transit request failed, retry with plain text"` | Encryption header missing.                                                                                                                                     |
| `400`      | `"Invalid JSON payload"`                                        | Malformed request.                                                                                                                                             |

# SwiftUI Support

Use CleverTap within SwiftUI apps by bridging lifecycle hooks and SDK initialization with AppDelegate.

## Integration

CleverTap iOS SDK can be integrated into the SwiftUI sample app. SwiftUI provides a way to use AppDelegate within the SwiftUI life cycle by using `@UIApplicationDelegateAdaptor`. Create a file, e.g.,`AppDelegate.swift` then create a class of AppDelegate and attach it with the main entry point struct by using the `@UIApplicationDelegateAdaptor` property wrapper. For more information, refer to [sample app](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftUIStarter/SwiftUIStarter/SwiftUIStarterApp.swift).

```swift
import UserNotifications
import CleverTapSDK

class AppDelegate: NSObject, UIApplicationDelegate, UNUserNotificationCenterDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        registerForPush()
        CleverTap.setDebugLevel(2)
        CleverTap.autoIntegrate()
        CleverTap.sharedInstance()?.enableDeviceNetworkInfoReporting(true)
        return true
    }
    
    func registerForPush() {
        // Register for Push notifications
        UNUserNotificationCenter.current().delegate = self
        // request Permissions
        UNUserNotificationCenter.current().requestAuthorization(options: [.sound, .badge, .alert], completionHandler: {granted, error in
            if granted {
                DispatchQueue.main.async {
                    UIApplication.shared.registerForRemoteNotifications()
                }
            }
        })
    }
}
```

Refer to the AppDelegate in your entry point struct like so:

```swift
import SwiftUI

@main
struct SwiftUIStarterApp: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

## App Inbox in SwiftUI

App Inbox controller can be added using `UIViewControllerRepresentable` and its callback methods can be used using `makeCoordinator` method. For more information, refer to  [sample app](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftUIStarter/SwiftUIStarter/CTAppInboxRepresentable.swift).

```swift
struct CTAppInboxRepresentable: UIViewControllerRepresentable {
    func makeUIViewController(context: Context) -> CleverTapInboxViewController {
        let style = CleverTapInboxStyleConfig()
        let inboxVC: CleverTapInboxViewController = (CleverTap.sharedInstance()?.newInboxViewController(with: style, andDelegate: context.coordinator))!
        return inboxVC
    }
    
    func updateUIViewController(_ uiViewController: CleverTapInboxViewController, context: Context) {
        // Updates the state of the specified view controller with new information from SwiftUI.
    }
    
    func makeCoordinator() -> CTCallbackCoordinator {
        // Callback class
    }
}
```

## Track Screen Views in SwiftUI

There is no direct replacement for `viewDidLoad()` method in SwiftUI, but we can achieve the same behavior using `onAppear` modifier. For more information, refer to [sample app](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftUIStarter/SwiftUIStarter/CTViewModifier.swift).

```swift
#if canImport(SwiftUI)
import SwiftUI
import CleverTapSDK

@available(iOS 13, *)
internal struct CTViewModifier: ViewModifier {
    @State private var viewDidLoad = false
    let screenName: String

    func body(content: Content) -> some View {
        content.onAppear {
            if viewDidLoad == false {
                // `viewDidLoad` eqivalent in SwiftUI
                viewDidLoad = true
                // Record any CleverTap events here.
                CleverTap.sharedInstance()?.recordEvent(screenName)
            }
        }
    }
}

@available(iOS 13, *)
public extension View {
    func recordScreenView(screenName: String) -> some View {
        self.modifier(CTViewModifier(screenName: screenName))
    }
}
#endif

```

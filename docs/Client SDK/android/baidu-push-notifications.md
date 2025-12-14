---
title: CleverTap Baidu Push Integration
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
China has the world’s largest number of smartphone users.   However, Android users in China can only receive push notifications powered by Baidu Cloud Push. Baidu has an Android app marketplace with over 195M daily users and is the largest source of app downloads for Android in China. In the rest of the world, Google’s Firebase Cloud Messaging (FCM) is used to send push notifications to Android smartphones. For global businesses with apps that want to target users in China, it is critical for them to enable Baidu push. 

Follow to steps to enable push notifications for Baidu:

# Register as a Baidu Developer

The first step to access the Baidu cloud push is to register as a developer. 

# Enter console and create an application

Log in to the Baidu account that has been registered as a developer on the homepage, and then click Get Started or "My Console" in the upper right corner to enter the [push console](http://push.baidu.com/console/app).

Entering the push console will enter the application list page. The application list page shows all the applications that you have created in the Baidu Developer Center. If you have configured applications in the old management console, the new management console can be used directly. For newly created or never-configured applications, you must perform application configuration before you can send messages and other operations.

When you need to create a new application, click the Create Application button on the application list page:

![135](https://files.readme.io/e1f2b63-Baidu_Push_Create_Application_button.png "Baidu_Push_Create_Application_button.png")

Then name your app. The application name can be a combination of Chinese, numbers or English letters, with a maximum of 32 characters. Note that the application name complies with the relevant laws and regulations and complies with the "cloud push" [developer service agreement](http://push.baidu.com/doc/guide/licence).\
After successful creation, you can configure the application immediately, or you can enter it later through the link in the application list.

![623](https://files.readme.io/124a0d2-Baidu_Push_name_the_app.png "Baidu_Push_name_the_app.png")

# Application configuration - Android

If you are configuring an application for the first time, you need to select the application platform (Android). After saving this configuration, the application platform cannot be changed, please proceed with caution.

Android applications need to configure the package name of the application. The package name requirements are as follows

* Can only contain uppercase letters (A to Z), lowercase letters (a to z), numbers and underscores, Chinese, and can be separated by periods
* Each logo must start with a letter or underscore or Chinese
* Contains at least two identifiers or at least one period
* Cannot contain Java reserved words
* There can be only one period between two identifiers

# Get the app's ApiKey / SecretKey

ApiKey is the application identifier. During the SDK invocation process, an application is uniquely identified. SecretKey is the Token when calling the API. It is used to verify the validity of the request. Please keep confidential. ApiKey / SecretKey can be found in the application details page after the application is created.

# Integrate Baidu Push SDK

Download the latest Android SDK archive and unpack, increase Baidu cloud push functionality in an existing or new project.

## Import cloud push jar package and so file

Copy all files in the unzipped libs folder to the libs folder of your project. If there are no other .so files in your project, it is recommended to copy only the armeabi folder. If other .so files are used in your project, you only need to copy the .so files in the corresponding directory. If your Android development environment is Android Studio, create a new directory named jniLibs in the "src / main" directory of the project, and copy the files in the libs folder to the "jniLibs" directory. As shown below -

![364](https://files.readme.io/a113f33-Baidu_Push_Jar_Package_and_SO_file.png "Baidu_Push_Jar_Package_and_SO_file.png")

## Configure the AndroidManifest file

In the AndroidManifest.xml file of the current project, add permissions and declaration information

```xml
<!-- Push service 运行需要的权限 -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
<uses-permission android:name="android.permission.WRITE_SETTINGS" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

<!-- 适配Android N系统必需的ContentProvider写权限声明，写权限包含应用包名-->
<uses-permission android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YourPackageName" />
<permission
        android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YourPackageName"
        android:protectionLevel="signature">
</permission>

<!-- push service start -->
<!-- 用于接收系统消息以保证PushService正常运行 -->
<receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
    android:process=":bdservice_v1" >
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
        <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
        <!-- 以下四项为可选的action声明，可大大提高service存活率和消息到达速度 -->
        <action android:name="android.intent.action.MEDIA_MOUNTED" />
        <action android:name="android.intent.action.USER_PRESENT" />
        <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
        <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
    </intent-filter>
</receiver>
<!-- Push服务接收客户端发送的各种请求-->
<receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
    android:process=":bdservice_v1" >
    <intent-filter>
        <action android:name="com.baidu.android.pushservice.action.METHOD" />
        <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
    </intent-filter>
    <intent-filter>
        <action android:name="android.intent.action.PACKAGE_REMOVED" />
        <data android:scheme="package" />
    </intent-filter>
</receiver>
<service android:name="com.baidu.android.pushservice.PushService" android:exported="true"
    android:process=":bdservice_v1" >
    <intent-filter >
            <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
    </intent-filter>
</service>

<!-- 4.4版本新增的CommandService声明，提升小米和魅族手机上的实际推送到达率 -->
<service android:name="com.baidu.android.pushservice.CommandService"
    android:exported="true" />

<!-- 适配Android N系统必需的ContentProvider声明，写权限包含应用包名-->
<provider
    android:name="com.baidu.android.pushservice.PushInfoProvider"
    android:authorities="YourPackageName.bdpush"
    android:writePermission="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YourPackageName"
    android:protectionLevel = "signature"
    android:exported="true" />

<!-- 在百度开发者中心查询应用的API Key -->
<meta-data android:name="api_key" android:value="YOUR_API_KEY" />

<!-- push结束 -->
```

After configuring the Manifest file, please `YourPackageName` replace it with your own package name and `YOUR_API_KEY`  with your API key.

## Launch cloud push

In the onCreate function of the main activity of the current project, add the following code:

```java JAVA
PushManager.startWork(getApplicationContext(), PushConstants.LOGIN_TYPE_API_KEY,
                Utils.getMetaValue(YourMainActivity.this, "api_key"));
```
```kotlin Kotlin
PushManager.startWork(getApplicationContext(), PushConstants.LOGIN_TYPE_API_KEY,
                      Utils.getMetaValue(this@YourMainActivity, "api_key"))
```

## Custom callback class

Create a new class in the current project, right-click and select new-> Class, fill in the class name of the class that receives cloud push callback information and push notification arrival information, and click Finish to create the class file (in this example, PushTestReceiver is used).

![303](https://files.readme.io/66a9e55-Baidu_Push_custom_callback_class.png "Baidu_Push_custom_callback_class.png")

Open the newly created class and inherit PushMessageReceiver. At this time, the class name is red, move to the class name, click Add unimplemented methods, reload all callback functions, and print the corresponding information, as shown in the figure:

![862](https://files.readme.io/f074d90-Baidu_Push_custom_callback_class_open.png "Baidu_Push_custom_callback_class_open.png")

In the AndroidManifest.xml file of the current project, add custom Receiver information as follows:

```xml
<!-- 此处Receiver名字修改为当前包名路径 -->
<receiver android:name="YourPackageName.PushTestReceiver">
    <intent-filter>
        <!-- 接收push消息 -->
        <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
        <!-- 接收bind、setTags等method的返回结果-->
        <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
        <!-- 接收通知点击事件，和通知自定义内容 -->
        <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
    </intent-filter>
</receiver>
```

## Pass Token to CleverTap

In the onBind method add the method to pass the token/Channel ID to CleverTap

```java JAVA
/**
     * 调用PushManager.startWork后，sdk将对push
     * server发起绑定请求，这个过程是异步的。绑定请求的结果通过onBind返回。 如果您需要用单播推送，需要把这里获取的channel
     * id和user id上传到应用server中，再调用server接口用channel id和user id给单个手机或者用户推送。
     *
     * @param context   BroadcastReceiver的执行Context
     * @param errorCode 绑定接口返回值，0 - 成功
     * @param appid     应用id。errorCode非0时为null
     * @param userId    应用user id。errorCode非0时为null
     * @param channelId 应用channel id。errorCode非0时为null
     * @param requestId 向服务端发起的请求id。在追查问题时有用；
     * @return none
     */
    @Override
    public void onBind(Context context, int errorCode, String appid,
                       String userId, String channelId, String requestId) {
        CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(context);
        if (cleverTapAPI != null) {
            cleverTapAPI.pushBaiduRegistrationId(channelId,true);
        }
    }
```
```text Kotlin
/**
 * 调用PushManager.startWork后，sdk将对push
 * server发起绑定请求，这个过程是异步的。绑定请求的结果通过onBind返回。 如果您需要用单播推送，需要把这里获取的channel
 * id和user id上传到应用server中，再调用server接口用channel id和user id给单个手机或者用户推送。
 *
 * @param context BroadcastReceiver的执行Context
 * @param errorCode 绑定接口返回值，0 - 成功
 * @param appid 应用id。errorCode非0时为null
 * @param userId 应用user id。errorCode非0时为null
 * @param channelId 应用channel id。errorCode非0时为null
 * @param requestId 向服务端发起的请求id。在追查问题时有用；
 * @return none
 */
fun onBind(context:Context, errorCode:Int, appid:String,
           userId:String, channelId:String, requestId:String) {
  val cleverTapAPI = CleverTapAPI.getDefaultInstance(context)
  if (cleverTapAPI != null)
  {
    cleverTapAPI.pushBaiduRegistrationId(channelId, true)
  }
}
```

## Receive Push Notifications from CleverTap

In the onMessageReceived method, write the following code for CleverTap to render Push Notification and raise Notification Viewed event - 

```java JAVA
/**
     * 接收透传消息的函数。
     *
     * @param context             上下文
     * @param message             推送的消息
     * @param customContentString 自定义内容,为空或者json字符串
     */
    @Override
    public void onMessage(Context context, String message,
                          String customContentString) {
        try {
            Bundle extras = com.clevertap.android.sdk.Utils.stringToBundle(message);
            CleverTapAPI.createNotification(context,extras);
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
```
```kotlin Kotlin
/**
 * 接收透传消息的函数。
 *
 * @param context 上下文
 * @param message 推送的消息
 * @param customContentString 自定义内容,为空或者json字符串
 */
fun onMessage(context:Context, message:String,
              customContentString:String) {
  try
  {
    val extras = com.clevertap.android.sdk.Utils.stringToBundle(message)
    CleverTapAPI.createNotification(context, extras)
  }
  catch (e:JSONException) {
    e.printStackTrace()
  }
}
```

# Update API Key / Secret Key in Settings Dashboard

![1454](https://files.readme.io/4bc639c-16.png "16.png")

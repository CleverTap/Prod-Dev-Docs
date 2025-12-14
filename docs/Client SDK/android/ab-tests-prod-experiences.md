---
title: Product Experiences_with AB Tests_Remove
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Product Experiences allow you to control and enhance the user experience for your products. 

The CleverTap Product Experiences comprise the following:

  * Product Config - Change the behavior and appearance of your app remotely without an update. This helps you to deliver in-app personalization experience to your app users and test their response. 
  * A/B Tests - Compare different versions of an app to each other and determine which one performs better. 
  * Feature Flags - Toggle a feature on and off.


#Implement Product Config/AB Tests

The steps to implement Product Config and AB Tests are common. 

##1. Get the Product Config/AB Tests object
The singleton object is used to store in-app default parameter values, fetch updated parameter values from the backend, and control when fetched values are made available to your app.

[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\n\nCTProductConfigController\nproductConfigInstance = cleverTapAPI.productConfig();\n\n",
      "language": "java"
    }
  ]
}
[/block]
##2. Set in-app default parameter values
You can set in-app default parameter values in the Product Config/AB Tests object so that your app behaves as intended before values are fetched from CleverTap, and so that default values are available if none are set on the dashboard.

Define a set of parameter names and default parameter values using a Map object or an XML resource file stored in your app's `res/xml` folder. 

Sample XML default:
[block:code]
{
  "codes": [
    {
      "code": "<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<!-- START xml_defaults -->\n<defaultsMap>\n    <entry>\n        <key>loading_phrase</key>\n        <value>Fetching config...</value>\n    </entry>\n    <entry>\n        <key>welcome_message_caps</key>\n        <value>false</value>\n    </entry>\n    <entry>\n        <key>welcome_message</key>\n        <value>Welcome to my awesome app!</value>\n    </entry>\n</defaultsMap>\n<!-- END xml_defaults -->",
      "language": "xml"
    }
  ]
}
[/block]
 
Add these values to the Product Config/AB Tests object using `setDefaults(int)` or `setDefaults(map)`, as shown:
[block:code]
{
  "codes": [
    {
      "code": "//Set defaults using XML resource file\ncleverTapAPI.productConfig().setDefaults(R.xml.product_config_defaults)\n//OR\n//Set defaults using Hashmap  \nHashMap<String, Object> map = new HashMap<>();\nmap.put(\"abc\", 623);\nmap.put(\"def\", \"624\");\nmap.put(\"fgh\", 625.0f);\nmap.put(\"mno\", 626L);\ncleverTapAPI.productConfig().setDefault(map);\n\n",
      "language": "java"
    }
  ]
}
[/block]
##3. Fetch and activate values
To fetch parameter values from CleverTap, call the `fetch()` method. Any values that you set on the dashboard are fetched and stored in the Product Config object.
To make fetched parameter values available to your app, call the `activate()` method.

For cases where you want to fetch and activate values in one call, you can use a `fetchAndActivate()` request to fetch values from CleverTap and make them available to the app:
          
 ###Fetch:
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().fetch();\n\n",
      "language": "java"
    }
  ]
}
[/block]
 By default, the fetch calls are throttled, which is controlled from the CleverTap servers as well as SDK. To know more see the [Throttling section](https://developer.clevertap.com/docs/product-experiences#section-5-throttling)

 In specific cases if you require to call fetch without the default throttling, you can use `fetch(intervalInSeconds)`.

[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().fetch(60*10);\n\n",
      "language": "java"
    }
  ]
}
[/block]

Here the interval is used to decide whether we can fetch or not. You can keep the interval as per your requirement, however, you should keep it high to avoid making frequent requests to the CleverTap servers.

  ###Activate:
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().activate();\n\n",
      "language": "java"
    }
  ]
}
[/block]
###FetchAndActivate:
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().fetchAndActivate();\n\n",
      "language": "java"
    }
  ]
}
[/block]
    
##4. Register Listener to Receive Product Config Callbacks:
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\ncleverTapAPI.setCTProductConfigListener(new CTProductConfigListener() {\n           @Override\n           public void onInit() {\n\t\t\t//Must Call activate if you want to apply the last fetched values on init every time.\n\t\t\tcleverTapAPI.productConfig().activate();\n           }\n\n           @Override\n           public void onFetched() {\n\n           }\n\n           @Override\n           public void onActivated() {\n\n           }\n       });\n       \n       ",
      "language": "java"
    }
  ]
}
[/block]
You must call `activate()` if you want to apply the last fetched values inside the `onInit()` method every time.

##5. Throttling

If an app fetches too many times in a short time period, fetch calls are throttled.
Throttling is controlled from the CleverTap servers as well from the SDK.

The maximum value of the two will be considered to make the next fetch request.

You can set the next fetch interval from the SDK below. 
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().setMinimumFetchIntervalInSeconds\n(60*10);\n\n\n",
      "language": "java"
    }
  ]
}
[/block]
##6. Get parameter values to use in your app

Now you can get parameter values from the Product Config object. If you set values on the dashboard, fetch them, and then activate them, those values are available to your app. Otherwise, you get the in-app parameter values configured using `setDefaults(int)` or `setDefaults(map)`. To get these values, call the method listed below that maps to the data type expected by your app, providing the parameter key as an argument:
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().getBoolean(\"key_bool\")\ncleverTapAPI.productConfig().getDouble(\"key_double\")\ncleverTapAPI.productConfig().getLong(\"key_long\")\ncleverTapAPI.productConfig().getString(\"key_string\")\ncleverTapAPI.productConfig().getString(\"key_json\")\n\n",
      "language": "java"
    }
  ]
}
[/block]
The Supported data types are: Boolean, Double, Long, String, and JSON.

##Considerations: 
JSON operations will be similar to that of String.
The getters return the default values of the type in case we don’t find any value for a key.
[block:parameters]
{
  "data": {
    "h-0": "Type",
    "h-1": "Default Value",
    "0-0": "String or JSON",
    "0-1": "“”",
    "1-0": "Long",
    "1-1": "0L",
    "2-0": "Double",
    "2-1": "0.0D",
    "3-0": "Boolean",
    "3-1": "false"
  },
  "cols": 2,
  "rows": 4
}
[/block]
##7. Resetting the Configs
If you want to reset the settings and/or already stored data to start fresh you can do that via `reset()` method.
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().reset()\n\n",
      "language": "java"
    }
  ]
}
[/block]
##8. Last Response Time Stamp
You can always get the timestamp of the configuration fetched last time. 
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.productConfig().getLastFetchTimeStampInMillis()\n\n",
      "language": "java"
    }
  ]
}
[/block]
#Implement Feature Flag

Feature flags allow you to toggle a feature on and off via the CleverTap Backend.

##1. Get the Feature Flag singleton object
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\nCTFeatureFlagsController\nfeatureFlagInstance = cleverTapAPI.featureFlag();\n\n",
      "language": "java"
    }
  ]
}
[/block]
 Feature flags are automatically fetched every time a new app session is created.

After the flags are fetched you can get it via the getter methods.

##2. Register the Listener

Register the listener to receive the callbacks whenever feature flags are fetched and stored.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\n\n\ncleverTapAPI.setCTFeatureFlagsListener(new CTFeatureFlagsListener() {\n   @Override\n   public void featureFlagsUpdated() {\n      // start using the latest Feature Flags values here.\n   }\n});\n\n\n",
      "language": "java"
    }
  ]
}
[/block]
##3. Getting the Feature Flags:

 To get the value of a feature simply call `get(key, defaultValue)`. The default value is the value that will be returned in case there are no feature flags with the key.
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.featureFlag().get(\"Key\", false);\n\n",
      "language": "java"
    }
  ]
}
[/block]
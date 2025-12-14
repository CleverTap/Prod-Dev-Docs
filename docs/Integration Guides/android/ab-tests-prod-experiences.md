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

# Implement Product Config/AB Tests

The steps to implement Product Config and AB Tests are common. 

## 1. Get the Product Config/AB Tests object

The singleton object is used to store in-app default parameter values, fetch updated parameter values from the backend, and control when fetched values are made available to your app.

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());

CTProductConfigController
productConfigInstance = cleverTapAPI.productConfig();
```

## 2. Set in-app default parameter values

You can set in-app default parameter values in the Product Config/AB Tests object so that your app behaves as intended before values are fetched from CleverTap, and so that default values are available if none are set on the dashboard.

Define a set of parameter names and default parameter values using a Map object or an XML resource file stored in your app's `res/xml` folder. 

Sample XML default:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- START xml_defaults -->
<defaultsMap>
    <entry>
        <key>loading_phrase</key>
        <value>Fetching config...</value>
    </entry>
    <entry>
        <key>welcome_message_caps</key>
        <value>false</value>
    </entry>
    <entry>
        <key>welcome_message</key>
        <value>Welcome to my awesome app!</value>
    </entry>
</defaultsMap>
<!-- END xml_defaults -->
```

Add these values to the Product Config/AB Tests object using `setDefaults(int)` or `setDefaults(map)`, as shown:

```java
//Set defaults using XML resource file
cleverTapAPI.productConfig().setDefaults(R.xml.product_config_defaults)
//OR
//Set defaults using Hashmap  
HashMap<String, Object> map = new HashMap<>();
map.put("abc", 623);
map.put("def", "624");
map.put("fgh", 625.0f);
map.put("mno", 626L);
cleverTapAPI.productConfig().setDefault(map);
```

## 3. Fetch and activate values

To fetch parameter values from CleverTap, call the `fetch()` method. Any values that you set on the dashboard are fetched and stored in the Product Config object.\
To make fetched parameter values available to your app, call the `activate()` method.

For cases where you want to fetch and activate values in one call, you can use a `fetchAndActivate()` request to fetch values from CleverTap and make them available to the app:

 \###Fetch:

```java
cleverTapAPI.productConfig().fetch();
```

 By default, the fetch calls are throttled, which is controlled from the CleverTap servers as well as SDK. To know more see the [Throttling section](https://developer.clevertap.com/docs/product-experiences#section-5-throttling)

 In specific cases if you require to call fetch without the default throttling, you can use `fetch(intervalInSeconds)`.

```java
cleverTapAPI.productConfig().fetch(60*10);
```

Here the interval is used to decide whether we can fetch or not. You can keep the interval as per your requirement, however, you should keep it high to avoid making frequent requests to the CleverTap servers.

  \###Activate:

```java
cleverTapAPI.productConfig().activate();
```

### FetchAndActivate:

```java
cleverTapAPI.productConfig().fetchAndActivate();
```

## 4. Register Listener to Receive Product Config Callbacks:

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
cleverTapAPI.setCTProductConfigListener(new CTProductConfigListener() {
           @Override
           public void onInit() {
			//Must Call activate if you want to apply the last fetched values on init every time.
			cleverTapAPI.productConfig().activate();
           }

           @Override
           public void onFetched() {

           }

           @Override
           public void onActivated() {

           }
       });
```

You must call `activate()` if you want to apply the last fetched values inside the `onInit()` method every time.

## 5. Throttling

If an app fetches too many times in a short time period, fetch calls are throttled.\
Throttling is controlled from the CleverTap servers as well from the SDK.

The maximum value of the two will be considered to make the next fetch request.

You can set the next fetch interval from the SDK below. 

```java
cleverTapAPI.productConfig().setMinimumFetchIntervalInSeconds
(60*10);
```

## 6. Get parameter values to use in your app

Now you can get parameter values from the Product Config object. If you set values on the dashboard, fetch them, and then activate them, those values are available to your app. Otherwise, you get the in-app parameter values configured using `setDefaults(int)` or `setDefaults(map)`. To get these values, call the method listed below that maps to the data type expected by your app, providing the parameter key as an argument:

```java
cleverTapAPI.productConfig().getBoolean("key_bool")
cleverTapAPI.productConfig().getDouble("key_double")
cleverTapAPI.productConfig().getLong("key_long")
cleverTapAPI.productConfig().getString("key_string")
cleverTapAPI.productConfig().getString("key_json")
```

The Supported data types are: Boolean, Double, Long, String, and JSON.

## Considerations: 

JSON operations will be similar to that of String.\
The getters return the default values of the type in case we don’t find any value for a key.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Type
      </th>

      <th>
        Default Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        String or JSON
      </td>

      <td>
        “”
      </td>
    </tr>

    <tr>
      <td>
        Long
      </td>

      <td>
        0L
      </td>
    </tr>

    <tr>
      <td>
        Double
      </td>

      <td>
        0.0D
      </td>
    </tr>

    <tr>
      <td>
        Boolean
      </td>

      <td>
        false
      </td>
    </tr>
  </tbody>
</Table>

## 7. Resetting the Configs

If you want to reset the settings and/or already stored data to start fresh you can do that via `reset()` method.

```java
cleverTapAPI.productConfig().reset()
```

## 8. Last Response Time Stamp

You can always get the timestamp of the configuration fetched last time. 

```java
cleverTapAPI.productConfig().getLastFetchTimeStampInMillis()
```

# Implement Feature Flag

Feature flags allow you to toggle a feature on and off via the CleverTap Backend.

## 1. Get the Feature Flag singleton object

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
CTFeatureFlagsController
featureFlagInstance = cleverTapAPI.featureFlag();
```

 Feature flags are automatically fetched every time a new app session is created.

After the flags are fetched you can get it via the getter methods.

## 2. Register the Listener

Register the listener to receive the callbacks whenever feature flags are fetched and stored.

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());


cleverTapAPI.setCTFeatureFlagsListener(new CTFeatureFlagsListener() {
   @Override
   public void featureFlagsUpdated() {
      // start using the latest Feature Flags values here.
   }
});
```

## 3. Getting the Feature Flags:

 To get the value of a feature simply call `get(key, defaultValue)`. The default value is the value that will be returned in case there are no feature flags with the key.

```java
cleverTapAPI.featureFlag().get("Key", false);
```

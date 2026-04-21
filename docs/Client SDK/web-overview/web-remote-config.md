---
title: Web Product Experiences
excerpt: Learn how to set up Remote Config for your Web application.
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

CleverTap's Remote Config feature lets you modify your website's functionality and appearance without deploying new code by using variables. The CleverTap Web SDK lets you define client-side variables with default values; you can override them from the CleverTap dashboard and deliver them to users at runtime. Variables can also be used in A/B tests to selectively roll out features to a percentage of your users.

To use Remote Config, define variables in your code using the SDK, then sync them to the dashboard as a one-time setup step. Once synced, you can manage and update variable values directly from the CleverTap dashboard without any code changes. 

<Callout icon="📘" theme="info">
  Note

  Syncing variables to the dashboard is a one-time setup step intended for development only and should not be performed in production.
</Callout>

## Prerequisite

To use the Remote Config feature, check that  you are using the following:

* Web SDK v1.7.0 or higher for String, Number, and Boolean variables.
* Web SDK v1.14.0 or higher for JSON object and File variables.

## Supported Variable Types

Currently, CleverTap Web SDK supports the following variable types:

* Number
* String
* Boolean
* JSON object (v1.14.0+)
* File (v1.14.0+)

# Update to CleverTap Initialize Method

The `syncVariables()` method requires the account token. This field is included in `clevertap.init()` method. Henceforth, tokens are required to maintain parity, as they are mandatory in mobile platforms.

## Using CleverTap as a Script

Add the account token to the CleverTap object.

```javascript
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[], region: REGION, token: TOKEN};
 //replace TOKEN with Account Token value from your Dashboard -> Settings page
 //replace the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard > Settings page.
clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
clevertap.privacy.push({optOut: false}); //set the flag to true, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false}); //set the flag to true, if the user agrees to share their IP data
 (function () {
		 var wzrk = document.createElement('script');
		 wzrk.type = 'text/javascript';
		 wzrk.async = true;
		 wzrk.src = 'https://d2r1yp2w7bby2u.cloudfront.net/js/clevertap.min.js';
		 var s = document.getElementsByTagName('script')[0];
		 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

## Using CleverTap as NPM Package

The account token is added as the fourth parameter in the `clevertap.init()` method.

```javascript
clevertap.init('ACCOUNT_ID', 'REGION', 'TARGET_DOMAIN', 'TOKEN') 
```

# Define Variables

The Web SDK exposes a method `defineVariable` to set the default value of the variables.

```javascript
const var1 = clevertap.defineVariable("var_string", "str")  
const var2 = clevertap.defineVariable("var_boolean", true)
```

# Set Up Callback URLs

CleverTap SDK provides several callbacks for the developer to receive feedback from the SDK. The following are a few instances where developers can set up callbacks:

* [Status of Fetch Variables request](https://staging.docs.dev.clevertap.net/docs/web-remote-config#status-of-fetch-variables-request)
* [Change of individual variable value](https://staging.docs.dev.clevertap.net/docs/web-remote-config#change-in-individual-variable-value)
* [Initialize all variables or change variables](https://staging.docs.dev.clevertap.net/docs/web-remote-config#initialize-all-variables-or-change-all-variable-values)

## Status of Fetch Variables Request

The `fetchVariables()` method provides feedback to ensure the variables were successfully retrieved from the server.

```javascript
clevertap.fetchVariables(  
  // Success callback function  
  () => { console.log('Fetch successful'); },  
);
```

> 📘 Retrieving Variables
>
> The retrieval of variables is limited to only one callback, meaning that any subsequent calls to the method overrides the existing callback. As a result, only the most recent callback is triggered when the variables are retrieved.

## Change in Individual Variable Value

The individual callback is registered per variable. It is called on the app start or whenever the variable value changes.

```javascript
const valueChangedCallback = (changedVariable) => {  
  console.log(`Value changed for variable '${changedVariable.name}'`);  
  // Add your callback logic here  
};

// Add the callback to the variable using addValueChangedCallback method  
var1.addValueChangedCallback(valueChangedCallback);
```

## Initialize All Variables or Change All Variable Values

The global callback registered on the CleverTap instance is called when variables are initialized with a value or changed with a new server value.

```javascript
cleverTap.addVariablesChangedCallback(() => {  
    // implement  
});

cleverTap.addOneTimeVariablesChangedCallback(() => {  
    // implement  
});
```

# Sync Defined Variables

<br />

After defining your variables in the code, you must send/sync variables to the server. The log level must be set to 4. From the CleverTap dashboard, mark a required CleverTap user profile as a test profile. For more information, refer to [Mark a User Profile as a Test Profile](https://docs.clevertap.com/docs/user-profiles#mark-a-user-profile-as-a-test-profile).

After marking the profile as a test profile, you must sync the app variables in DEBUG mode:

```javascript
clevertap.syncVariables() 

or  

clevertap.syncVariables(  
  // Success callback function  
  () => { console.log('Sync successful'); },

  // Failure callback function  
  () => { console.log('Sync failed'); }  
);
```

> 📘 Key Points
>
> In case, if another user profile has already created a draft in the dashboard, syncing variables will fail to prevent overriding their changes. Before syncing variables again, either publish or dismiss the existing draft. However, if you have created a draft previously, you can override it using the sync method to optimize the integration experience.

# Fetch Variables During a Session

During a session, you can fetch the updated values for your CleverTap variables from the server. The fetched data goes into local storage under the **WZRK_PE** key.

The `fetchVariable()` method is triggered once when the page count is 1.

```javascript
clevertap.fetchVariables()  

or  

clevertap.fetchVariables(  
  // Success callback function  
  () => { console.log('Fetch successful'); },  
);
```

## Accessing the Variable Value

To access the value from `Var` instance, you can use several methods on the `Var` instance as shown in the following code:

```javascript
variable.getdefaultValue(); // returns default value  
variable.getValue(); // returns current value
```

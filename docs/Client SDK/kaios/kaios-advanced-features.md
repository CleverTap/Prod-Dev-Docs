---
title: KaiOS Debug
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Debugging

By default, CleverTap logs are set to CleverTap.Loglevels.INFO. During development, we recommend that you set the SDK to DEBUG mode in order to log warnings or other important messages to the Android logging system. This can be done by setting the debug level to CleverTap.Loglevels.DEBUG. If you want to disable CleverTap logs for the production environment, you can set the debug level to CleverTap.Loglevels.OFF.

```javascript
clevertap.setLogLevel(clevertap.logLevels.DEBUG);
```

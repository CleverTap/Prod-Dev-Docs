---
title: Segment - Android Install
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
You can download the artifacts for android from the [CleverTap Android Segment](https://github.com/CleverTap/clevertap-segment-android) page.

For integration, download the [Segment Analytics tools](https://github.com/segmentio/analytics-android).

# Installation

To install the Segment-CleverTap integration, simply add this line to your gradle file:

```groovy
implementation 'com.clevertap.android:clevertap-segment-android:+'
```

# Usage

After adding the dependency, you must register the integration. To do this, import the CleverTap integration:

```java JAVA
import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration;
```
```kotlin Kotlin
import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration
```

And add the following line:

```java JAVA
Analytics analytics = new Analytics.Builder(this, "write_key")
                .use(CleverTapIntegration.FACTORY)
                .build();
```
```kotlin Kotlin
var analytics = Analytics.Builder(this, "write_key")
.use(CleverTapIntegration.FACTORY)
.build()
```

Check the [Segment documentation](https://segment.com/docs/connections/destinations/catalog/clevertap/) for more information.

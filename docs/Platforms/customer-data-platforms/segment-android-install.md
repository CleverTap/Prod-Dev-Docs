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
[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.clevertap.android:clevertap-segment-android:+'\n",
      "language": "groovy"
    }
  ]
}
[/block]

# Usage
After adding the dependency, you must register the integration. To do this, import the CleverTap integration:
[block:code]
{
  "codes": [
    {
      "code": "import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration;\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "import com.segment.analytics.android.integrations.clevertap.CleverTapIntegration",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
And add the following line:

[block:code]
{
  "codes": [
    {
      "code": "Analytics analytics = new Analytics.Builder(this, \"write_key\")\n                .use(CleverTapIntegration.FACTORY)\n                .build();",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "var analytics = Analytics.Builder(this, \"write_key\")\n.use(CleverTapIntegration.FACTORY)\n.build()",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
Check the [Segment documentation](https://segment.com/docs/connections/destinations/catalog/clevertap/) for more information.
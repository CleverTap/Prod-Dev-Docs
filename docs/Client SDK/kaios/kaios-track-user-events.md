---
title: KaiOS Track User Events
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
#Track User Events

CleverTap provides the ability to track custom events that are specific to your website. For example, if you are an e-commerce company, you most likely want to track what products were viewed. 

1. Track custom events by calling clevertap.event.push("EVENT_NAME").

Here is an example how to track that a product was viewed:
[block:code]
{
  "codes": [
    {
      "code": "clevertap.event.push(\"Product Viewed\");",
      "language": "javascript"
    }
  ]
}
[/block]
2. In addition to tracking an event, you can add any key/value pairs as properties on the event which lets you create more specific segments and targeted campaigns. 
[block:code]
{
  "codes": [
    {
      "code": "clevertap.event.push(\"Product Viewed\", {\n  \"Product name\":\"Casio Chronograph Watch\",\n  \"Category\":\"Mens Accessories\",\n  \"Price\":59.99,\n});",
      "language": "javascript"
    }
  ]
}
[/block]
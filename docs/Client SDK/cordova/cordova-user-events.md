---
title: Cordova User Events
excerpt: Understand how to record user events with Cordova.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## User Events

## Record an event with event data
[block:code]
{
  "codes": [
    {
      "code": "// event with properties\nvar props = { 'Name': 'XYZ', 'Price': 123 }\nCleverTap.recordEventWithNameAndProps(\"Cordova Event\", props)",
      "language": "javascript"
    }
  ]
}
[/block]
## Record an event without event data
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.recordEventWithName(\"Cordova Event\")",
      "language": "javascript"
    }
  ]
}
[/block]
## Record Charged event
[block:code]
{
  "codes": [
    {
      "code": "var chargeDetails = { 'totalValue': 20, \n                      'category': 'books', \n                      'purchase_date': new Date()\n                    }\nvar items = [\n{ 'title': 'book1', 'published_date': new Date('2010-12-12T06:35:31'), 'author': 'ABC' },\n{ 'title': 'book2', 'published_date': new Date('2020-12-12T06:35:31'), 'author': 'DEF') },\n{ 'title': 'book3', 'published_date': new Date('2000-12-12T06:35:31'), 'author': 'XYZ' }]\n\nCleverTap.recordChargedEventWithDetailsAndItems(chargeDetails, items);",
      "language": "javascript"
    }
  ]
}
[/block]
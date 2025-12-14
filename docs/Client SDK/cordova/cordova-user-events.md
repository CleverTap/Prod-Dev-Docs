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

```javascript
// event with properties
var props = { 'Name': 'XYZ', 'Price': 123 }
CleverTap.recordEventWithNameAndProps("Cordova Event", props)
```

## Record an event without event data

```javascript
CleverTap.recordEventWithName("Cordova Event")
```

## Record Charged event

```javascript
var chargeDetails = { 'totalValue': 20, 
                      'category': 'books', 
                      'purchase_date': new Date()
                    }
var items = [
{ 'title': 'book1', 'published_date': new Date('2010-12-12T06:35:31'), 'author': 'ABC' },
{ 'title': 'book2', 'published_date': new Date('2020-12-12T06:35:31'), 'author': 'DEF') },
{ 'title': 'book3', 'published_date': new Date('2000-12-12T06:35:31'), 'author': 'XYZ' }]

CleverTap.recordChargedEventWithDetailsAndItems(chargeDetails, items);
```

---
title: React Native User Events
excerpt: Understand how to record user events with React Native.
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

CleverTap.recordEvent('Product Viewed', props)
```

## Record an event without event data

```javascript
// event without properties
CleverTap.recordEvent('Product Viewed')
```

> 📘 Nested Objects Support
>
> To update profile properties using nested objects, refer to [Ingesting Nested Objects via CleverTap SDK](https://staging.docs.dev.clevertap.net/docs/ingesting-nested-objects-via-clevertap-sdk).

## Record Charged event

```javascript
var chargeDetails = { 'totalValue': 20, 
                      'category': 'books', 
                      'purchase_date': new Date()
                    }
var items = [
{ 'title': 'book1', 'published_date': new Date('2010-12-12T06:35:31'), 'author': 'ABC' },
{ 'title': 'book2', 'published_date': new Date('2020-12-12T06:35:31'), 'author': 'DEF'},
{ 'title': 'book3', 'published_date': new Date('2000-12-12T06:35:31'), 'author': 'XYZ' }]

CleverTap.recordChargedEvent(chargeDetails, items);
```

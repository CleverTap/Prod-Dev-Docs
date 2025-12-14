---
title: Content Blocks API
excerpt: Learn how to use the Content Block API.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
A Content Block is a reusable block of content that can be used across different campaigns. It helps maintain a consistent experience for users as they view a consistent branding tone and colors across your messaging. This messaging can include text, images, or calls to action (CTAs) buttons that you can standardize across messages. By using standard messaging, marketers can maintain a clear tone and style, which builds trust and consistency in brand communications.

APIs can be used to create or retrieve content blocks at scale. A maximum of 3 concurrent requests can be made to these APIs.

Here are the API endpoints available to interact with the Content Block object:

- [Get Content Block List](doc:get-content-block-list-api)  
  The _Get Message Content Block List_ API gives you a list of all the available content blocks specified by the date range. 
- [Get Specific Content Block](doc:get-specific-content-block-api)  
  The Get Specific Content Block API enables you to get only a specific Content Block by their ID.
- [Create Content Block](doc:create-content-block-api)  
  The Create Content Block API allows you to create a content block with the specific content and the type of the Content Block, that is, HTML or Text.
- [Update Content Block](doc:update-content-block-api)  
  The Update Content Block API allows you to update existing content in a Content Block. For example, you can update the raw HTML in a content block using this API.
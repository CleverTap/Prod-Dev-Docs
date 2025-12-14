---
title: Sender ID in Manifest File
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
## Specify the Sender ID in manifest.json File

This section is applicable to Chrome and Firefox browsers.

> 📘 Legacy Protocol
>
> This step is only required if you are using the legacy protocol.

To register the service worker and subscribe for push notifications, you need to specify your sender ID inside a website manifest file.

If your website already has a manifest, add the following property to it. Your sender ID is your FCM project number (12-digit number)—not your API key.

```json
{
  "gcm_sender_id": "YOUR_SENDER_ID"
}
```

If you do not have a manifest file yet, create a new file called manifest.json and add it in the document root of your website. Ensure that you specify the "name" and "gcm\_sender\_id" parameters where "name" is the name of your brand (e.g. Acme Fashion).

Here is a sample manifest.json file:

```json
{
  "name": "Acme Fashion | Latest in fashion world",
  "gcm_sender_id": "1234567890" // replace with actual value
}
```

Then in all your HTML files, include the snippet below in the header:

```html
<head>
  <link rel="manifest" href="/manifest.json">
</head>
```

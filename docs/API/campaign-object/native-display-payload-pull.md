---
title: Pull Native Display API
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Overview

The Pull Native Display API lets you get the payload from CleverTap using the API.

# Base URL

[https://in1-paytm.api.clevertap.com/pull](https://in1-paytm.api.clevertap.com/pull)

# HTTP Method

POST

# Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap Account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap Account Passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/json.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

# Body Parameters

The body is uploaded as a JSON payload.\
Will return campaigns which have start time between the the date range.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        identity
      </td>

      <td>
        Identity of the user for whom the request is made.
      </td>

      <td>
        string
      </td>

      <td>
        [john@abc.com](mailto:john@abc.com)
      </td>
    </tr>

    <tr>
      <td>
        contexts
      </td>

      <td>
        Defines the context for the relevant engagement.
      </td>

      <td>
        mixed
      </td>

      <td>
        homescreen\_slot1
      </td>
    </tr>

    <tr>
      <td>
        device
      </td>

      <td>
        Operating system of the device. (Optional)
      </td>

      <td>
        string
      </td>

      <td>
        iOS
      </td>
    </tr>

    <tr>
      <td>
        wzrk\_info
      </td>

      <td>
        The user's internal identifier in CleverTap system
      </td>

      <td>
        string
      </td>

      <td>
        j25378
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```ruby
{
	"Identity":"k0911a@ct.com",
	"contexts":["ads"],
	"device":"iOS", // optional
  	"wzrk_info":{ // optional
       		"j":123 // this is the person's internal identifier in the CleverTap system.
    	}
}
```

# Example Request

URL: [https://in1-paytm.api.clevertap.com/pull](https://in1-paytm.api.clevertap.com/pull)\
Method: Post\
Headers:\
X-CleverTap-Account-Id\
X-CleverTap-Passcode

```curl
curl -X POST -d '{"Identity":"k0911a@ct.com","contexts":["ads"],"device":"iOS","wzrk_info":{"j":123}}'\ "https://in1-paytm.api.clevertap.com/pull" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

# Example Response

```json Custom Key value
{
    "context_screen": [
        {
            "type": "custom-key-value",
                "custom_kv": {
                    "key 3": "val 3"
                },
                "wzrk_info": {
                    "j": 75034,
                    "wzrk_id": "1601039214_20200929",
                    "wzrk_pivot": "Variant C"
                }

        }
    ],
    "status": "success"
}
```
```json Simple Banner
{
 "adUnit_notifs": {
   "context1": [
     {
       "type": "simple",
       "bg": "#E73333",
       "content": [
         {
           "key": 274792526,
           "message": {
             "text": "Message - Disp A personalisation: Ben Liquid: Ben",
             "color": "#000000"
           },
           "title": {
             "text": "Title [simple banner] - Disp A personalisation: Ben Liquid: Ben",
             "color": "#000000"
           },
           "action": {
             "url": {
               "android": {
                 "text": "http://cdn.branch.io/branchster-angular/?body_index=4&color_index=2&face_index=1&monster=true&monster_name=DLE_Deep_link/Ben"
               },
               "ios": {
                 "text": ""
               }
             },
             "hasUrl": true
           },
           "media": {
             "url": "https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/a6c1773c5bdc4429889129c8e4804df0.jpeg?v=1601018521",
             "poster": "",
             "key": "a6c1773c5bdc4429889129c8e4804df0",
             "content_type": "image/jpeg",
             "processing": false
           },
           "icon": {}
         }
       ],
       "custom_kv": {
         "k1": "Ben",
         "k2": "v2"
       },
       "wzrk_info": {
         "j": 6811,
         "wzrk_id": "1601025975_20200925",
         "wzrk_pivot": "Variant A"
       }
     }
   ]
 },
 "status": "success"
}
```
```json Carousel Banner
{
 "adUnit_notifs": {
   "context1": [
     {
       "type": "carousel",
       "bg": "#ffffff",
       "content": [
         {
           "key": 731699747,
           "message": {
             "text": "Message [Carausal banner] - Disp C personalisation: Ben Liquid: Ben",
             "color": "#000000"
           },
           "title": {
             "text": "Title [Carausal banner] - Disp C personalisation: Ben liquid: Ben",
             "color": "#000000"
           },
           "action": {
             "url": {
               "android": {
                 "text": "http://cdn.branch.io/branchster-angular/?body_index=4&color_index=2&face_index=1&monster=true&monster_name=DLE_Deep_link/Ben"
               },
               "ios": {
                 "text": ""
               }
             },
             "hasUrl": true
           },
           "media": {
             "url": "https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/220f75e9f3194fa49b8e7f4d54c7208d.jpeg?v=1601018791",
             "poster": "",
             "key": "220f75e9f3194fa49b8e7f4d54c7208d",
             "content_type": "image/jpeg",
             "processing": false
           },
           "icon": {}
         },
         {
           "key": 8485087218,
           "message": {
             "text": "Message",
             "color": "#000000"
           },
           "title": {
             "text": "Title",
             "color": "#000000"
           },
           "action": {
             "url": {
               "android": {
                 "text": ""
               },
               "ios": {
                 "text": ""
               }
             },
             "hasUrl": true
           },
           "media": {},
           "icon": {}
         }
       ],
       "custom_kv": {
         "k1": "Ben",
         "k2": "v2"
       },
       "wzrk_info": {
         "j": 6811,
         "wzrk_id": "1601025975_20200925",
         "wzrk_pivot": "Variant C"
       }
     }
   ]
 },
 "status": "success"
}
```
```json Banner with icon
{
 "adUnit_notifs": {
   "context1": [
     {
       "type": "message-icon",
       "bg": "#ffffff",
       "content": [
         {
           "key": 2903378248,
           "message": {
             "text": "Message - Banner with icon. Disp E personalisation: Ben Liquid: Ben",
             "color": "#000000"
           },
           "title": {
             "text": "Title - Banner with icon. Disp E personalisation: Ben Liquid: Ben",
             "color": "#000000"
           },
           "action": {
             "url": {
               "android": {
                 "text": "http://cdn.branch.io/branchster-angular/?body_index=4&color_index=2&face_index=1&monster=true&monster_name=DLE_Deep_link/Ben"
               },
               "ios": {
                 "text": ""
               }
             },
             "hasUrl": true
           },
           "media": {
             "url": "https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/fa2bafec26ba4b10a0eee46037f6e8df.jpeg?v=1601030433",
             "poster": "",
             "key": "fa2bafec26ba4b10a0eee46037f6e8df",
             "content_type": "image/jpeg",
             "processing": false
           },
           "icon": {
             "url": "https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/53a6602a054148ddbbc53def942d1b8a.jpeg?v=1601030407",
             "poster": "",
             "key": "53a6602a054148ddbbc53def942d1b8a",
             "content_type": "image/jpeg",
             "processing": false
           }
         }
       ],
       "custom_kv": {
         "k1": "Ben",
         "k2": "v2"
       },
       "wzrk_info": {
         "j": 6811,
         "wzrk_id": "1601030553_20200925",
         "wzrk_pivot": "Variant E"
       }
     }
   ]
 },
 "status": "success"
}
```

> 📘 Note
>
> There may be some extra keys in the response. These are internal keys that are used by the CleverTap SDK. You will not need them unless you have integrated the CleverTap SDK.

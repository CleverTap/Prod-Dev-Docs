---
title: getMessages API
excerpt: >-
  Learn how you can retrieve messages from the CleverTap Inbox channel using
  getMessagesAPI.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The `getMessages API` lets you retrieve the messages from the CleverTap Inbox channel. Whenever this API is called, it returns the list of available messages for the user.

# Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/1/inbox/getMessages>

### Region

Refer to [Region](doc:common-api-components#region) for more details.

# HTTP Method

POST

# Headers

The following headers are required when you use CleverTap APIs:

```json
X-CleverTap-Account-Id:<encoded-account-id>
X-CleverTap-Passcode:<account-passcode>
Content-Type:application/json; charset=utf-8
Accept-Encoding: gzip
```

The `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` authenticate the request.

| Header                 | Description                                                 | Type   | Example Value                        |
| :--------------------- | :---------------------------------------------------------- | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                                  | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap Account Passcode.                            | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | The request content-type is always set to application/json. | string | "Content-Type: application/json"     |
| Accept-Encoding        | The type of encoding used when fetching the messages.       | string | gzip                                 |

# Body Parameters

The following are the body parameters included in the JSON response payload:

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "status",
    "0-1": "Indicates whether the request was successfully processed. This value is either set as success or fail.",
    "0-2": "string",
    "0-3": "fail",
    "1-0": "totalAvailableMessages",
    "1-1": "Indicates the number of messages present for the user in CleverTap Inbox.",
    "1-2": "numeric",
    "1-3": "5",
    "2-0": "wzrk_id",
    "2-1": "Indicates the internal key to identify the campaign. The customer must retain this parameter to generate click events.",
    "2-2": "string",
    "2-3": "12345679_20240130",
    "3-0": "wzrk_pivot",
    "3-1": "Indicates the internal key to identify the campaign variants. The customer must retain this parameter to generate click events.",
    "3-2": "string",
    "3-3": "wzrk_default  \n(when the default variant is rolled out to the user)",
    "4-0": "messages",
    "4-1": "Contains a list of messages for the user. Each message stores message-related data and the message content. If the user has no messages, the list is empty.",
    "4-2": "array",
    "4-3": "\\[  \n    {  \n      \"wzrk_id\": \"1707198495_20240206\",  \n      \"wzrk_pivot\": \"wzrk_default\",  \n      \"isRead\": false,  \n      \"messageId\": 1,  \n      \"createdAt\": 1707198495,  \n      \"msg\": {  \n        \"type\": \"simple\",  \n        \"content\": [  \n          {  \n            \"key\": 8716500285,  \n            \"message\": {  \n              \"text\": \"Message\",  \n              \"color\": \"#434761\"  \n            },  \n            \"title\": {  \n              \"text\": \"Title\",  \n              \"color\": \"#434761\"  \n            },  \n            \"icon\": {}  \n          }  \n        ],  \n        \"bg\": \"#ffffff\",  \n        \"orientation\": \"l\",  \n        \"tags\": [  \n          \"Test\"  \n        ],  \n        \"enableTags\": true  \n      }  \n    }  \n  ]",
    "5-0": "createdAt",
    "5-1": "Indicates the timestamp when the message was created for the user.",
    "5-2": "timestamp",
    "5-3": "1707198495",
    "6-0": "isRead",
    "6-1": "Indicates if the message is read.",
    "6-2": "Boolean",
    "6-3": "true",
    "7-0": "messageId",
    "7-1": "Uniquely identifies the message.",
    "7-2": "numeric",
    "7-3": "1",
    "8-0": "msg",
    "8-1": "Stores the message content along with the custom key-value pairs.",
    "8-2": "string",
    "8-3": "\"msg\": {  \n        \"type\": \"simple\",  \n        \"content\": \\[  \n          {  \n            \"key\": 8716500285,  \n            \"message\": {  \n              \"text\": \"Message\",  \n              \"color\": \"#434761\"  \n            },  \n            \"title\": {  \n              \"text\": \"Title\",  \n              \"color\": \"#434761\"  \n            },  \n            \"icon\": {}  \n          }",
    "9-0": "type",
    "9-1": "Indicates the type of message template selected under _WHAT_ section of the Campaign dashboard.",
    "9-2": "string",
    "9-3": "carousel-image",
    "10-0": "custom_kv",
    "10-1": "Stores the Custom KV pairs specified under the _WHAT_ section of the campaign.",
    "10-2": "object",
    "10-3": "{  \n          \"android\": [  \n            {  \n              \"key\": \"all1\",  \n              \"value\": {  \n                \"text\": \"vall1\"  \n              }  \n            },  \n            {  \n              \"key\": \"android1\",  \n              \"value\": {  \n                \"text\": \"v4\"  \n              }  \n            }  \n          ],  \n          \"ios\": [  \n            {  \n              \"key\": \"all1\",  \n              \"value\": {  \n                \"text\": \"vall1\"  \n              }  \n            },  \n            {  \n              \"key\": \"ios1\",  \n              \"value\": {  \n                \"text\": \"v4\"  \n              }  \n            }  \n          ],  \n          \"web\": \\[  \n            {  \n              \"key\": \"all1\",  \n              \"value\": {  \n                \"text\": \"vall1\"  \n              }  \n            }",
    "11-0": "msg.content.message",
    "11-1": "Stores the text specified in the message.",
    "11-2": "object",
    "11-3": "\"message\": {  \n              \"text\": \"Message1\",  \n              \"color\": \"#434761\"  \n            },",
    "12-0": "msg.content.title",
    "12-1": "Stores the text specified in the title.",
    "12-2": "object",
    "12-3": "\"title\": {  \n              \"text\": \"Title1\",  \n              \"color\": \"#434761\"  \n            }",
    "13-0": "msg.content.action",
    "13-1": "Stores actions specified under the _WHAT_ section of the campaign  \n  \n<li> url: Stores the following parts of the WHAT section: Call to Action Url for iOS, Android, and Web. </li> <li> links: </li> <li> media: Stores the media link.</li>Stores the additional links added at the bottom of the message for iOS, Android, and Web.</li>  \n  \n<br />",
    "13-2": "string",
    "13-3": "\"action\": {  \n              \"hasUrl\": false,  \n              \"hasLinks\": false,  \n              \"url\": {  \n                \"android\": {  \n                  \"text\": \"\"  \n                },  \n                \"ios\": {  \n                  \"text\": \"\"  \n                },  \n                \"web\": {  \n                  \"text\": \"\"  \n                }  \n              }"
  },
  "cols": 4,
  "rows": 14,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


# Example Request

Here is an example cURL request to the getMessages API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl --location --request POST 'https://in1.api.clevertap.com/1/inbox/getMessages' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": "test@stockx.com"
}'
```
```ruby
require "uri"
require "json"
require "net/http"

url = URI("https://in1.api.clevertap.com/1/inbox/getMessages")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "YOUR_ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "YOUR_ACCOUNT_PASSCODE"
request["X-CleverTap-Token"] = "YOUR_ACCOUNT_TOKEN"
request["Content-Type"] = "application/json"
request["Accept-Encoding"] = " gzip, deflate"
request.body = JSON.dump({
  "userId": "stockx@test.com"
})

response = http.request(request)
puts response.read_body
```
```python
import requests
import json

url = "https://in1.api.clevertap.com/1/inbox/getMessages"

payload = json.dumps({
  "userId": "stockx@test.com"
})
headers = {
  'X-CleverTap-Account-Id': 'YOUR_ACCOUNT_ID',
  'X-CleverTap-Passcode': 'YOUR_ACCOUNT_PASSCODE',
  'X-CleverTap-Token': 'YOUR_ACCOUNT_TOKEN',
  'Content-Type': 'application/json',
  'Accept-Encoding': ' gzip, deflate'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://in1.api.clevertap.com/1/inbox/getMessages',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS =>'{
  "userId": "stockx@test.com"
}',
  CURLOPT_HTTPHEADER => array(
    'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID',
    'X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE',
    'X-CleverTap-Token: YOUR_ACCOUNT_TOKEN',
    'Content-Type: application/json',
    'Accept-Encoding:  gzip, deflate'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```
```node
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://in1.api.clevertap.com/1/inbox/getMessages',
  'headers': {
    'X-CleverTap-Account-Id': 'YOUR_ACCOUNT_ID',
    'X-CleverTap-Passcode': 'YOUR_ACCOUNT_PASSCODE',
    'X-CleverTap-Token': 'YOUR_ACCOUNT_TOKEN',
    'Content-Type': 'application/json',
    'Accept-Encoding': ' gzip, deflate'
  },
  body: JSON.stringify({
    "userId": "stockx@test.com"
  })

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
```

# Example Response

```json Simple Message Template
{
  "status": "success",
  "totalAvailableMessages": 1,
  "messages": [
    {
      "wzrk_id": "1707198495_20240206",
      "wzrk_pivot": "wzrk_default",
      "isRead": false,
      "messageId": 1,
      "createdAt": 1707198495,
      "msg": {
        "type": "simple",
        "content": [
          {
            "key": 8716500285,
            "message": {
              "text": "Message",
              "color": "#434761"
            },
            "title": {
              "text": "Title",
              "color": "#434761"
            },
            "action": {
              "hasUrl": true,
              "hasLinks": true,
              "url": {
                "android": {
                  "text": "https://www.androidurl.com"
                },
                "ios": {
                  "text": "https://www.iosurl.com"
                },
                "web": {
                  "text": "https://www.weburl.com"
                }
              },
              "links": [
                {
                  "type": "copy",
                  "text": "CopyToClipboard",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": "Copy Text"
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {}
                },
                {
                  "type": "url",
                  "text": "OpenUrl",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": "https://www.androidurl.com"
                    },
                    "ios": {
                      "text": "https://www.iosurl.com"
                    },
                    "web": {
                      "text": "https://www.weburl.com"
                    }
                  },
                  "kv": {}
                },
                {
                  "type": "kv",
                  "text": "Custom KV",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    }
                  },
                  "kv": {
                    "k1": "v1",
                    "k2": "v2"
                  }
                }
              ]
            },
            "media": {
              "url": "https://www.resorceurl.com",
              "poster": "",
              "key": "b20ea1c782944ee094b950248decffd5",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": false
            },
            "icon": {}
          }
        ],
        "bg": "#ffffff",
        "orientation": "l",
        "tags": [
          "Test"
        ],
        "enableTags": true,
        "custom_kv": {
          "android": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "android1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "ios": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "ios1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "web": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "web1",
              "value": {
                "text": "v4"
              }
            }
          ]
        }
      }
    }
  ]
}
```
```json Message With Icon Template
{
  "status": "success",
  "totalAvailableMessages": 1,
  "messages": [
    {
      "wzrk_id": "1707198495_20240206",
      "wzrk_pivot": "wzrk_default",
      "isRead": false,
      "messageId": 1,
      "createdAt": 1707198495,
      "msg": {
        "bg": "#ffffff",
        "type": "message-icon",
        "content": [
          {
            "key": 247408732,
            "message": {
              "text": "Message",
              "color": "#434761"
            },
            "title": {
              "text": "Title",
              "color": "#434761"
            },
            "action": {
              "hasUrl": true,
              "hasLinks": true,
              "url": {
                "android": {
                  "text": "https://www.androidurl.com"
                },
                "ios": {
                  "text": "https://www.iosurl.com"
                },
                "web": {
                  "text": "https://www.weburl.com"
                }
              },
              "links": [
                {
                  "type": "url",
                  "text": "Open URL",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": "https://www.androidurl.com"
                    },
                    "ios": {
                      "text": "https://www.iosurl.com"
                    },
                    "web": {
                      "text": "https://www.weburl.com"
                    }
                  },
                  "kv": {}
                },
                {
                  "type": "kv",
                  "text": "Custom KV",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {
                    "kv1": "v1",
                    "k2": "v2"
                  }
                },
                {
                  "type": "copy",
                  "text": "Copy To Clipboard",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": "Text"
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {}
                }
              ]
            },
            "media": {
              "url": "https://www.resourceurl.com",
              "poster": "",
              "key": "1373dd6ae2484564bde2a793962b36b6",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": true
            },
            "icon": {
              "url": "https://www.resourceurl.com",
              "poster": "",
              "key": "032ff414eb3a4b039a9c82f96b49c27d",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": false
            }
          }
        ],
        "custom_kv": {
          "android": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "android1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "ios": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "ios1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "web": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "web1",
              "value": {
                "text": "v4"
              }
            }
          ]
        },
        "enableTags": false,
        "orientation": "l",
        "tags": [
          "Test"
        ]
      }
    }
  ]
}
```
```json Carousel Message Template
{
  "status": "success",
  "totalAvailableMessages": 1,
  "messages": [
    {
      "wzrk_id": "1707198495_20240206",
      "wzrk_pivot": "wzrk_default",
      "isRead": false,
      "messageId": 1,
      "createdAt": 1707198495,
      "msg": {
        "type": "carousel",
        "content": [
          {
            "key": 7841707692,
            "message": {
              "text": "Message1",
              "color": "#434761"
            },
            "title": {
              "text": "Title1",
              "color": "#434761"
            },
            "action": {
              "hasUrl": false,
              "hasLinks": false,
              "url": {
                "android": {
                  "text": ""
                },
                "ios": {
                  "text": ""
                },
                "web": {
                  "text": ""
                }
              },
              "links": [
                {
                  "type": "copy",
                  "text": "Link",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {}
                }
              ]
            },
            "media": {
              "url": "https://www.resourceurl.com",
              "poster": "",
              "key": "53ac9ce4777c4d99b0ca0b44e750c4fe",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": false
            },
            "icon": {}
          },
          {
            "key": 5779755833,
            "message": {
              "text": "Message2",
              "color": "#434761"
            },
            "title": {
              "text": "Title2",
              "color": "#434761"
            },
            "action": {
              "hasUrl": true,
              "hasLinks": false,
              "url": {
                "android": {
                  "text": "https://www.androidurl.com"
                },
                "ios": {
                  "text": "https://www.iosurl.com"
                },
                "web": {
                  "text": "https://www.weburl.com"
                }
              },
              "links": [
                {
                  "type": "copy",
                  "text": "Link",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {}
                }
              ]
            },
            "media": {
              "url": "https://www.resourceurl.com",
              "poster": "",
              "key": "6c47ea18aa0545599eaa237cf883d185",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": false
            },
            "icon": {}
          }
        ],
        "bg": "#ffffff",
        "orientation": "l",
        "tags": [
          "Test"
        ],
        "enableTags": true,
        "custom_kv": {
          "android": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "android1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "ios": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "ios1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "web": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "web1",
              "value": {
                "text": "v4"
              }
            }
          ]
        }
      }
    }
  ]
}
```
```json Image Only Carousel Template
{
  "status": "success",
  "totalAvailableMessages": 1,
  "messages": [
    {
      "wzrk_id": "1707198495_20240206",
      "wzrk_pivot": "wzrk_default",
      "isRead": false,
      "messageId": 1,
      "createdAt": 1707198495,
      "msg": {
        "type": "carousel-image",
        "content": [
          {
            "key": 1314265955,
            "message": {
              "text": "Message",
              "color": "#434761"
            },
            "title": {
              "text": "Title",
              "color": "#434761"
            },
            "action": {
              "hasUrl": true,
              "hasLinks": false,
              "url": {
                "android": {
                  "text": "https://www.androidurl.com"
                },
                "ios": {
                  "text": "https://www.iosurl.com"
                },
                "web": {
                  "text": "https://www.weburl.com"
                }
              },
              "links": [
                {
                  "type": "copy",
                  "text": "Link",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {}
                }
              ]
            },
            "media": {
              "url": "https://www.resourseurl.com",
              "poster": "",
              "key": "ca16928737744423912740dadd9f5370",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": false
            },
            "icon": {}
          },
          {
            "key": 6785095013,
            "message": {
              "text": "Message",
              "color": "#434761"
            },
            "title": {
              "text": "Title",
              "color": "#434761"
            },
            "action": {
              "hasUrl": true,
              "hasLinks": false,
              "url": {
                "android": {
                  "text": "https://www.androidurl.com"
                },
                "ios": {
                  "text": "https://www.iosurl.com"
                },
                "web": {
                  "text": "https://www.weburl.com"
                }
              },
              "links": [
                {
                  "type": "copy",
                  "text": "Link",
                  "color": "#007bff",
                  "bg": "#ffffff",
                  "copyText": {
                    "text": ""
                  },
                  "url": {
                    "android": {
                      "text": ""
                    },
                    "ios": {
                      "text": ""
                    },
                    "web": {
                      "text": ""
                    }
                  },
                  "kv": {}
                }
              ]
            },
            "media": {
              "url": "https://www.resourseurl.com",
              "poster": "",
              "key": "aa1eb5301f5143f38f9b8ab8e8c31b29",
              "content_type": "image/jpeg",
              "filename": "",
              "processing": true
            },
            "icon": {}
          }
        ],
        "bg": "#ffffff",
        "orientation": "l",
        "tags": [
          "Test"
        ],
        "enableTags": true,
        "custom_kv": {
          "android": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "android1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "ios": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "ios1",
              "value": {
                "text": "v4"
              }
            }
          ],
          "web": [
            {
              "key": "all1",
              "value": {
                "text": "vall1"
              }
            },
            {
              "key": "web1",
              "value": {
                "text": "v4"
              }
            }
          ]
        }
      }
    }
  ]
}
```

If there is an error in fetching the messages, you receive the following response:

```json
{
    "status": "fail",
    "error": "<error description>",
    "code": "<error code>"
}
```

For more information about different errors you might encounter, refer to [Inbox API Errors](doc:inbox-api-errors).
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
https://in1-paytm.api.clevertap.com/pull

# HTTP Method
POST

# Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "0-0": "X-CleverTap-Account-Id",
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
# Body Parameters
The body is uploaded as a JSON payload.
Will return campaigns which have start time between the the date range.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "identity",
    "0-1": "Identity of the user for whom the request is made.",
    "0-2": "string",
    "0-3": "john@abc.com",
    "1-0": "contexts",
    "1-1": "Defines the context for the relevant engagement.",
    "1-2": "mixed",
    "1-3": "homescreen_slot1",
    "2-0": "device",
    "3-0": "wzrk_info",
    "2-1": "Operating system of the device. (Optional)",
    "2-3": "iOS",
    "2-2": "string",
    "3-1": "The user's internal identifier in CleverTap system",
    "3-2": "string",
    "3-3": "j25378"
  },
  "cols": 4,
  "rows": 4
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"Identity\":\"k0911a@ct.com\",\n\t\"contexts\":[\"ads\"],\n\t\"device\":\"iOS\", // optional\n  \t\"wzrk_info\":{ // optional\n       \t\t\"j\":123 // this is the person's internal identifier in the CleverTap system.\n    \t}\n}\n",
      "language": "ruby"
    }
  ]
}
[/block]
# Example Request

URL: https://in1-paytm.api.clevertap.com/pull
Method: Post
Headers: 
X-CleverTap-Account-Id
X-CleverTap-Passcode
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"Identity\":\"k0911a@ct.com\",\"contexts\":[\"ads\"],\"device\":\"iOS\",\"wzrk_info\":{\"j\":123}}'\\ \"https://in1-paytm.api.clevertap.com/pull\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    }
  ]
}
[/block]
# Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"context_screen\": [\n        {\n            \"type\": \"custom-key-value\",\n                \"custom_kv\": {\n                    \"key 3\": \"val 3\"\n                },\n                \"wzrk_info\": {\n                    \"j\": 75034,\n                    \"wzrk_id\": \"1601039214_20200929\",\n                    \"wzrk_pivot\": \"Variant C\"\n                }\n\n        }\n    ],\n    \"status\": \"success\"\n}\n\n",
      "language": "json",
      "name": "Custom Key value"
    },
    {
      "code": "{\n \"adUnit_notifs\": {\n   \"context1\": [\n     {\n       \"type\": \"simple\",\n       \"bg\": \"#E73333\",\n       \"content\": [\n         {\n           \"key\": 274792526,\n           \"message\": {\n             \"text\": \"Message - Disp A personalisation: Ben Liquid: Ben\",\n             \"color\": \"#000000\"\n           },\n           \"title\": {\n             \"text\": \"Title [simple banner] - Disp A personalisation: Ben Liquid: Ben\",\n             \"color\": \"#000000\"\n           },\n           \"action\": {\n             \"url\": {\n               \"android\": {\n                 \"text\": \"http://cdn.branch.io/branchster-angular/?body_index=4&color_index=2&face_index=1&monster=true&monster_name=DLE_Deep_link/Ben\"\n               },\n               \"ios\": {\n                 \"text\": \"\"\n               }\n             },\n             \"hasUrl\": true\n           },\n           \"media\": {\n             \"url\": \"https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/a6c1773c5bdc4429889129c8e4804df0.jpeg?v=1601018521\",\n             \"poster\": \"\",\n             \"key\": \"a6c1773c5bdc4429889129c8e4804df0\",\n             \"content_type\": \"image/jpeg\",\n             \"processing\": false\n           },\n           \"icon\": {}\n         }\n       ],\n       \"custom_kv\": {\n         \"k1\": \"Ben\",\n         \"k2\": \"v2\"\n       },\n       \"wzrk_info\": {\n         \"j\": 6811,\n         \"wzrk_id\": \"1601025975_20200925\",\n         \"wzrk_pivot\": \"Variant A\"\n       }\n     }\n   ]\n },\n \"status\": \"success\"\n}\n",
      "language": "json",
      "name": "Simple Banner"
    },
    {
      "code": "{\n \"adUnit_notifs\": {\n   \"context1\": [\n     {\n       \"type\": \"carousel\",\n       \"bg\": \"#ffffff\",\n       \"content\": [\n         {\n           \"key\": 731699747,\n           \"message\": {\n             \"text\": \"Message [Carausal banner] - Disp C personalisation: Ben Liquid: Ben\",\n             \"color\": \"#000000\"\n           },\n           \"title\": {\n             \"text\": \"Title [Carausal banner] - Disp C personalisation: Ben liquid: Ben\",\n             \"color\": \"#000000\"\n           },\n           \"action\": {\n             \"url\": {\n               \"android\": {\n                 \"text\": \"http://cdn.branch.io/branchster-angular/?body_index=4&color_index=2&face_index=1&monster=true&monster_name=DLE_Deep_link/Ben\"\n               },\n               \"ios\": {\n                 \"text\": \"\"\n               }\n             },\n             \"hasUrl\": true\n           },\n           \"media\": {\n             \"url\": \"https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/220f75e9f3194fa49b8e7f4d54c7208d.jpeg?v=1601018791\",\n             \"poster\": \"\",\n             \"key\": \"220f75e9f3194fa49b8e7f4d54c7208d\",\n             \"content_type\": \"image/jpeg\",\n             \"processing\": false\n           },\n           \"icon\": {}\n         },\n         {\n           \"key\": 8485087218,\n           \"message\": {\n             \"text\": \"Message\",\n             \"color\": \"#000000\"\n           },\n           \"title\": {\n             \"text\": \"Title\",\n             \"color\": \"#000000\"\n           },\n           \"action\": {\n             \"url\": {\n               \"android\": {\n                 \"text\": \"\"\n               },\n               \"ios\": {\n                 \"text\": \"\"\n               }\n             },\n             \"hasUrl\": true\n           },\n           \"media\": {},\n           \"icon\": {}\n         }\n       ],\n       \"custom_kv\": {\n         \"k1\": \"Ben\",\n         \"k2\": \"v2\"\n       },\n       \"wzrk_info\": {\n         \"j\": 6811,\n         \"wzrk_id\": \"1601025975_20200925\",\n         \"wzrk_pivot\": \"Variant C\"\n       }\n     }\n   ]\n },\n \"status\": \"success\"\n}\n",
      "language": "json",
      "name": "Carousel Banner"
    },
    {
      "code": "{\n \"adUnit_notifs\": {\n   \"context1\": [\n     {\n       \"type\": \"message-icon\",\n       \"bg\": \"#ffffff\",\n       \"content\": [\n         {\n           \"key\": 2903378248,\n           \"message\": {\n             \"text\": \"Message - Banner with icon. Disp E personalisation: Ben Liquid: Ben\",\n             \"color\": \"#000000\"\n           },\n           \"title\": {\n             \"text\": \"Title - Banner with icon. Disp E personalisation: Ben Liquid: Ben\",\n             \"color\": \"#000000\"\n           },\n           \"action\": {\n             \"url\": {\n               \"android\": {\n                 \"text\": \"http://cdn.branch.io/branchster-angular/?body_index=4&color_index=2&face_index=1&monster=true&monster_name=DLE_Deep_link/Ben\"\n               },\n               \"ios\": {\n                 \"text\": \"\"\n               }\n             },\n             \"hasUrl\": true\n           },\n           \"media\": {\n             \"url\": \"https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/fa2bafec26ba4b10a0eee46037f6e8df.jpeg?v=1601030433\",\n             \"poster\": \"\",\n             \"key\": \"fa2bafec26ba4b10a0eee46037f6e8df\",\n             \"content_type\": \"image/jpeg\",\n             \"processing\": false\n           },\n           \"icon\": {\n             \"url\": \"https://d2w5kj4l5j4fyn.cloudfront.net/dist/1585807616/i/53a6602a054148ddbbc53def942d1b8a.jpeg?v=1601030407\",\n             \"poster\": \"\",\n             \"key\": \"53a6602a054148ddbbc53def942d1b8a\",\n             \"content_type\": \"image/jpeg\",\n             \"processing\": false\n           }\n         }\n       ],\n       \"custom_kv\": {\n         \"k1\": \"Ben\",\n         \"k2\": \"v2\"\n       },\n       \"wzrk_info\": {\n         \"j\": 6811,\n         \"wzrk_id\": \"1601030553_20200925\",\n         \"wzrk_pivot\": \"Variant E\"\n       }\n     }\n   ]\n },\n \"status\": \"success\"\n}\n",
      "language": "json",
      "name": "Banner with icon"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "There may be some extra keys in the response. These are internal keys that are used by the CleverTap SDK. You will not need them unless you have integrated the CleverTap SDK."
}
[/block]
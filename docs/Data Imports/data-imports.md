---
title: Data Imports
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
The CleverTap enables you to import data such as events and profiles from third parties to the CleverTap platform. 

[block:api-header]
{
  "title": "Data Import"
}
[/block]
To perform a data import, complete the following steps:

1. Get the artifacts for data upload from the [CleverTap Repository](https://github.com/CleverTap/clevertap-data-upload). 

[block:callout]
{
  "type": "info",
  "body": "The CleverTap script runs in `Go`. Check that you have installed `Go` on your system.  To download and install go, see https://golang.org/dl/.",
  "title": "Note"
}
[/block]
2. Install the CleverTap upload tool. 
[block:code]
{
  "codes": [
    {
      "code": "go get github.com/CleverTap/clevertap-data-upload \ngo install github.com/CleverTap/clevertap-data-upload",
      "language": "text"
    }
  ]
}
[/block]
You can use the following arguments:
[block:code]
{
  "codes": [
    {
      "code": "-csv string               Absolute path to the csv file\n  \n  -id string                CleverTap Account ID\n  \n  -p string                 CleverTap Account Passcode\n  \n  -t string                 The type of data, either profile or event, defaults to profile (default \"profile\")\n  \n  -evtName string           Event name. Required only when uploading events. Each CSV file can only have one type of event\n  \n  -r string                 The account region, either eu, in, sk, or sg, defaults to eu (default \"eu\")\n  \n  -dryrun                   Do a dry run, process records but do not upload\n\n  -mixpanelSecret           Mixpanel API secret key\n\n  -startDate                Start date for exporting events from Mixpanel <yyyy-mm--dd>\n\n  -endDate                  End date for exporting events <yyyy-mm-dd>\n\n  -startTs                  Start timestamp for events upload in epoch",
      "language": "text"
    }
  ]
}
[/block]
3. Upload your data to CleverTap.

##From Mixpanel
You can import your data from Mixpanel directly to CleverTap. The script constantly checks your Mixpanel project for new data at an interval of 20 seconds. 

Example Events upload from Mixpanel:

[block:code]
{
  "codes": [
    {
      "code": "clevertap-data-upload -id=\"XXX-XXX-XXXX\" -p=\"XXX-XXX-XXXX\" -mixpanelSecret=\"<mp api secret>\" -t=\"event\" -startDate=\"<yyyy-mm-dd>\" -endDate=\"<yyyy-mm-dd>\"\n",
      "language": "text"
    }
  ]
}
[/block]
Example Profiles upload from Mixpanel:
[block:code]
{
  "codes": [
    {
      "code": "clevertap-data-upload -id=\"XXX-XXX-XXXX\" -p=\"XXX-XXX-XXXX\" -mixpanelSecret=\"<mixpanel secret key>\"\n",
      "language": "text"
    }
  ]
}
[/block]

##From other platforms such as Amplitude or any analytics tool

You can import data from any provider to CleverTap. You must first save the data from your platform in a CSV. You can then upload this CSV to CleverTap. 
Example Events upload from CSV:
[block:code]
{
  "codes": [
    {
      "code": "clevertap-data-upload -csv=\"/Users/name/Documents/in.csv\" -id=\"XXX-XXX-XXXX\" -p=\"XXX-XXX-XXXX\" -t=\"event\" -evtName=\"Product Viewed\"\n",
      "language": "text"
    }
  ]
}
[/block]
Example Profiles upload from CSV:
[block:code]
{
  "codes": [
    {
      "code": "clevertap-data-upload -csv=\"/Users/name/Documents/in.csv\" -id=\"XXX-XXX-XXXX\" -p=\"XXX-XXX-XXXX\"\n",
      "language": "text"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "For CSV uploads, you must include one of identity, objectId, FBID or GPID, in your data. Email addresses can serve as an identity value, but the key must be identity."
}
[/block]
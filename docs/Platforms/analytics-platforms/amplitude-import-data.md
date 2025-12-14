---
title: Amplitude - Import Data
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
You can import data such as events and profiles from Amplitude to the CleverTap platform. Perform a data import in three easy steps.

##Step 1: Get the artifacts for data upload from the [CleverTap Repository](https://github.com/CleverTap/clevertap-data-upload). 

[block:callout]
{
  "type": "info",
  "body": "The CleverTap script runs in `Go`. Check that you have installed `Go` on your system.  To download and install go, see https://golang.org/dl/.",
  "title": "Note"
}
[/block]
##Step 2: Install the CleverTap upload tool. 
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
      "code": "-csv string               Absolute path to the csv file\n  \n  -id string                CleverTap Account ID\n  \n  -p string                 CleverTap Account Passcode\n  \n  -t string                 The type of data, either profile or event, defaults to profile (default \"profile\")\n  \n  -evtName string           Event name. Required only when uploading events. Each CSV file can only have one type of event\n  \n  -r string                 The account region, either eu, in, sk, or sg, defaults to eu (default \"eu\")\n  \n  -dryrun                   Do a dry run, process records but do not upload\n\n  -startDate                Start date for exporting events from Mixpanel <yyyy-mm--dd>\n\n  -endDate                  End date for exporting events <yyyy-mm-dd>\n\n  -startTs                  Start timestamp for events upload in epoch",
      "language": "text"
    }
  ]
}
[/block]
##Step 3: Upload your Amplitude data to CleverTap

You can import data from Amplitude to CleverTap. You must first save the data from Amplitude in a CSV file. You can then upload this CSV file to CleverTap. 



Following is an example of columns in a CSV file for profiles:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/608c915-Amplitude_CSV_Events.png",
        "Amplitude_CSV_Events.png",
        609,
        146,
        "#f3f3f3"
      ],
      "border": true
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

Following is an example of columns in a CSV file for events:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/127c0c9-Amplitude_CSV_Events.png",
        "Amplitude_CSV_Events.png",
        625,
        173,
        "#f4f5f5"
      ],
      "border": true
    }
  ]
}
[/block]
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

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "For CSV uploads, you must include one of identity, objectId, FBID or GPID, in your data. Email addresses can serve as an identity value, but the key must be identity."
}
[/block]
---
title: Mixpanel - Import Data
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
You can import data such as events and profiles from Mixpanel to the CleverTap platform. Perform a data import in 3 easy steps.

##Step 1: Get the artifacts for data upload from the [CleverTap Repository](https://github.com/CleverTap/clevertap-data-upload). 

[block:callout]
{
  "type": "info",
  "body": "The CleverTap script runs in `Go`. Check that you have installed `Go` on your system.  To download and install go, see https://golang.org/dl/.",
  "title": "Note"
}
[/block]
##Step 2:Install the CleverTap upload tool. 
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
##Step 3: Upload your Mixpanel data to CleverTap

You can import your data from Mixpanel to CleverTap. The script constantly checks your Mixpanel project for new data at an interval of 20 seconds. 

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